---
title: Threading
author: Sander Stad
date: '2026-02-23'
categories:
  - Example
weight: 250
---

Threading allows you to run multiple pieces of work at the same time, making scripts faster when tasks are independent of each other. PowerShell offers several ways to achieve this, from simple background jobs to low-level runspaces.

---

# Background Jobs

The simplest way to run code in the background is `Start-Job`. Each job runs in a separate PowerShell process.

```powershell
$job = Start-Job -ScriptBlock {
    Start-Sleep -Seconds 2
    "Job completed"
}

# Do other work here while the job runs...
Write-Host "Waiting for job..."

# Block until the job finishes and retrieve output
Wait-Job -Job $job
Receive-Job -Job $job
Remove-Job -Job $job
```

Result:

```
Waiting for job...
Job completed
```

## Multiple Jobs

```powershell
$jobs = 1..5 | ForEach-Object {
    $number = $_
    Start-Job -ScriptBlock {
        Start-Sleep -Seconds (Get-Random -Minimum 1 -Maximum 3)
        "Job $using:number done"
    }
}

# Wait for all jobs to finish
$jobs | Wait-Job | Receive-Job
$jobs | Remove-Job
```

Result (order may vary):

```
Job 3 done
Job 1 done
Job 5 done
Job 2 done
Job 4 done
```

> Note: `Start-Job` spawns a new process for each job, which has significant overhead. Use Thread Jobs or Runspaces for better performance.

## Language Mode Error

If you see this error:

```
Start-Job: Cannot start job. The language mode for this session is incompatible with the system-wide language mode.
```

Your system enforces **Constrained Language Mode (CLM)** via AppLocker or Windows Defender Application Control (WDAC). Because `Start-Job` launches a new PowerShell process, that child process inherits the system-wide CLM policy and refuses to accept script blocks from a FullLanguage session.

You can check the current language mode with:

```powershell
$ExecutionContext.SessionState.LanguageMode
```

**Workarounds:**

The most practical alternative is `Start-ThreadJob` or `ForEach-Object -Parallel`, which run on threads inside the *same* process and are not subject to this restriction:

```powershell
# Use Start-ThreadJob instead
$job = Start-ThreadJob -ScriptBlock {
    Start-Sleep -Seconds 2
    "Job completed"
}

Wait-Job -Job $job
Receive-Job -Job $job
Remove-Job -Job $job
```

If you must use `Start-Job` in a CLM environment or on PowerShell 5.1, save the script to a `.ps1` file that is trusted under your AppLocker/WDAC policy and pass the file path with `-FilePath` instead of a script block:

```powershell
# myjob.ps1 must be in a path allowed by AppLocker/WDAC
Start-Job -FilePath "C:\Scripts\myjob.ps1"
```

---

# Thread Jobs

`Start-ThreadJob` (available in the `ThreadJob` module, included with PowerShell 7) runs jobs as threads instead of processes - much faster to start and with less memory overhead.

```powershell
# Install if not already available (PowerShell 5.1)
# Install-Module -Name ThreadJob

$jobs = 1..5 | ForEach-Object {
    $number = $_
    Start-ThreadJob -ScriptBlock {
        Start-Sleep -Milliseconds (Get-Random -Minimum 100 -Maximum 500)
        "Thread job $using:number done"
    }
}

$jobs | Wait-Job | Receive-Job
$jobs | Remove-Job
```

Result (order may vary):

```
Thread job 2 done
Thread job 4 done
Thread job 1 done
Thread job 5 done
Thread job 3 done
```

---

# ForEach-Object -Parallel (PowerShell 7+)

PowerShell 7 introduced the `-Parallel` parameter on `ForEach-Object`, which is the most concise way to parallelise a pipeline.

```powershell
1..5 | ForEach-Object -Parallel {
    Start-Sleep -Milliseconds (Get-Random -Minimum 100 -Maximum 500)
    "Item $_ processed"
} -ThrottleLimit 3
```

Result (order may vary):

```
Item 2 processed
Item 1 processed
Item 3 processed
Item 5 processed
Item 4 processed
```

The `-ThrottleLimit` controls how many threads run simultaneously (default is 5).

## Passing Variables into Parallel Blocks

Variables from the outer scope are not automatically available inside `-Parallel`. Use the `$using:` scope modifier.

```powershell
$prefix = "Result"
$multiplier = 10

1..5 | ForEach-Object -Parallel {
    "$using:prefix: $($_ * $using:multiplier)"
} -ThrottleLimit 5
```

Result:

```
Result: 10
Result: 20
Result: 30
Result: 40
Result: 50
```

---

# Runspaces

Runspaces are the lowest-level threading primitive in PowerShell. They are faster than jobs and give you the most control, but require more setup.

```powershell
$runspace = [System.Management.Automation.Runspaces.RunspaceFactory]::CreateRunspace()
$runspace.Open()

$ps = [System.Management.Automation.PowerShell]::Create()
$ps.Runspace = $runspace

$ps.AddScript({
    Start-Sleep -Milliseconds 500
    "Runspace completed"
}) | Out-Null

# Start asynchronously
$asyncResult = $ps.BeginInvoke()

# Do other work here...
Write-Host "Runspace is running asynchronously..."

# Wait and collect output
$output = $ps.EndInvoke($asyncResult)
$output

$ps.Dispose()
$runspace.Close()
```

Result:

```
Runspace is running asynchronously...
Runspace completed
```

---

# Runspace Pools (Advanced)

A `RunspacePool` limits the number of concurrent threads, preventing resource exhaustion when processing large collections.

```powershell
$maxThreads = 4
$pool = [System.Management.Automation.Runspaces.RunspaceFactory]::CreateRunspacePool(1, $maxThreads)
$pool.Open()

$jobs = [System.Collections.Generic.List[hashtable]]::new()

foreach ($i in 1..10) {
    $ps = [System.Management.Automation.PowerShell]::Create()
    $ps.RunspacePool = $pool

    $ps.AddScript({
        param($number)
        Start-Sleep -Milliseconds (Get-Random -Minimum 100 -Maximum 400)
        "Item $number processed on thread $(
            [System.Threading.Thread]::CurrentThread.ManagedThreadId
        )"
    }).AddArgument($i) | Out-Null

    $jobs.Add(@{
        PowerShell  = $ps
        AsyncResult = $ps.BeginInvoke()
    })
}

# Collect all results
foreach ($job in $jobs) {
    $job.PowerShell.EndInvoke($job.AsyncResult)
    $job.PowerShell.Dispose()
}

$pool.Close()
$pool.Dispose()
```

Result (order may vary; notice thread IDs are reused):

```
Item 2 processed on thread 12
Item 1 processed on thread 9
Item 4 processed on thread 11
Item 3 processed on thread 10
Item 6 processed on thread 12
Item 5 processed on thread 9
...
```

---

# Thread-Safe Collections

When multiple threads write to a shared collection, race conditions can occur. Use thread-safe types from `System.Collections.Concurrent`.

```powershell
$results = [System.Collections.Concurrent.ConcurrentBag[string]]::new()

1..10 | ForEach-Object -Parallel {
    $bag = $using:results
    $bag.Add("Item $_ finished")
} -ThrottleLimit 5

$results | Sort-Object
```

Result:

```
Item 1 finished
Item 2 finished
Item 3 finished
...
Item 10 finished
```

> Avoid writing to a plain `[System.Collections.Generic.List[T]]` or `@()` array from parallel threads - use `ConcurrentBag`, `ConcurrentQueue`, or `ConcurrentDictionary` instead.

---

# Summary

| Approach | Isolation | Speed | Best for |
|---|---|---|---|
| `Start-Job` | Process | Slow | Isolation, compatibility with PS 5.1 |
| `Start-ThreadJob` | Thread | Fast | General parallel tasks |
| `ForEach-Object -Parallel` | Thread | Fast | Simple pipeline parallelism (PS 7+) |
| Runspace | Thread | Fastest | Fine-grained control, high volume |
| RunspacePool | Thread | Fastest | Throttled high-volume processing |
