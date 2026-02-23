---
title: Regular Expressions
author: Sander Stad
date: '2026-02-23'
categories:
  - Example
weight: 145
---

Regular expressions (regex) are patterns used to match, search, and manipulate text. PowerShell has built-in support for regex through operators like `-match`, `-replace`, and `-split`, as well as the .NET `[regex]` class.

---

# Basic Examples

## Testing a match

The `-match` operator returns `$true` or `$false` depending on whether the pattern is found in the string.

```powershell
$email = "user@example.com"

if ($email -match "@") {
    Write-Host "Contains an @"
}

$result = "PowerShell 7" -match "\d"
$result
```

Result:

```
Contains an @
True
```

## Extracting a match with `$Matches`

When `-match` succeeds, PowerShell populates the automatic variable `$Matches` with the matched value and any capture groups.

```powershell
$version = "Version 7.4.1"

if ($version -match "(\d+\.\d+\.\d+)") {
    Write-Host "Found version: $($Matches[1])"
}
```

Result:

```
Found version: 7.4.1
```

## Replacing text with `-replace`

The `-replace` operator takes a pattern and a replacement string.

```powershell
$text = "Hello, my name is John Doe."

$result = $text -replace "John Doe", "Jane Smith"
$result
```

Result:

```
Hello, my name is Jane Smith.
```

You can also use a regex pattern in the replacement to strip unwanted characters:

```powershell
$dirty = "Ph0ne: +31 (0)6-12 34 56 78"

$clean = $dirty -replace "[^0-9+]", ""
$clean
```

Result:

```
+31061234567
```

## Splitting a string with `-split`

The `-split` operator accepts a regex pattern as the delimiter.

```powershell
$csv = "one,two,,three, four"

$parts = $csv -split ",\s*"
$parts
```

Result:

```
one
two

three
four
```

## Searching in files with `Select-String`

`Select-String` scans string input or files and returns lines that match a pattern.

```powershell
$lines = @(
    "ERROR: disk full",
    "INFO: backup started",
    "WARNING: low memory",
    "ERROR: connection lost"
)

$lines | Select-String -Pattern "^ERROR"
```

Result:

```
ERROR: disk full
ERROR: connection lost
```

---

# Advanced Examples

## Named capture groups

Named groups make patterns easier to read and maintain. Use `(?<name>...)` syntax to define a named group, then reference it via `$Matches.name`.

```powershell
$logLine = "2024-06-19 14:32:01 ERROR Something went wrong"

if ($logLine -match "(?<date>\d{4}-\d{2}-\d{2}) (?<time>\d{2}:\d{2}:\d{2}) (?<level>\w+) (?<message>.+)") {
    Write-Host "Date   : $($Matches.date)"
    Write-Host "Time   : $($Matches.time)"
    Write-Host "Level  : $($Matches.level)"
    Write-Host "Message: $($Matches.message)"
}
```

Result:

```
Date   : 2024-06-19
Time   : 14:32:01
Level  : ERROR
Message: Something went wrong
```

## Finding all matches with `[regex]::Matches()`

Unlike `-match`, which stops at the first match, `[regex]::Matches()` returns every match in the input.

```powershell
$text = "Call us at 555-1234 or 555-5678 for support."

$matches = [regex]::Matches($text, "\d{3}-\d{4}")

foreach ($m in $matches) {
    Write-Host "Found number: $($m.Value)"
}
```

Result:

```
Found number: 555-1234
Found number: 555-5678
```

## Using `[regex]::Replace()` with named groups

The .NET `[regex]::Replace()` method lets you restructure text using capture groups in the replacement string. This example evolves through three steps: positional groups, named groups, and what happens when you make a typo.

### Step 1 - Positional groups

The simplest approach uses numbered positional references (`$1`, `$2`, ...) to refer to capture groups.

```powershell
$dates = '29 July, 2014, 30 July, 2014, 31 July, 2014'

[regex]::Replace($dates, '\s*([^,]+)(,\s*)([^,]+),*\s*', '$1 = $3' + "`n")
```

Result:

```
29 July = 2014
30 July = 2014
31 July = 2014
```

This works, but `$1`, `$2`, `$3` tell you nothing about what each group captures. As patterns grow more complex, positional references become hard to follow.

### Step 2 - Named groups

Replace the numbered groups with named groups using `(?<name>...)` syntax. You then reference them as `${name}` in the replacement string.

```powershell
$dates = '29 July, 2014, 30 July, 2014, 31 July, 2014'

[regex]::Replace($dates, '\s*(?<day>[^,]+)(?<sep>,\s*)(?<year>[^,]+),*\s*', '${day} = ${year}' + "`n")
```

Result:

```
29 July = 2014
30 July = 2014
31 July = 2014
```

The pattern and replacement are now self-documenting: `${day}` and `${year}` make the intent obvious.

### Step 3 - What happens with a typo?

If you mistype a group name in the replacement string, .NET does not throw an error. Instead it outputs the literal text of the bad reference - making typos immediately visible in the output.

```powershell
$dates = '29 July, 2014, 30 July, 2014, 31 July, 2014'

# Note the typo: ${yr} instead of ${year}
[regex]::Replace($dates, '\s*(?<day>[^,]+)(?<sep>,\s*)(?<year>[^,]+),*\s*', '${day} = ${yr}' + "`n")
```

Result:

```
29 July = ${yr}
30 July = ${yr}
31 July = ${yr}
```

The replacement token `${yr}` appears verbatim in every line, making the mistake easy to spot - far better than silently producing wrong output.

## Validating an email address

A common use case for regex is input validation. The pattern below covers most standard email formats.

```powershell
function Test-EmailAddress {
    param([string]$Address)

    $pattern = '^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'

    if ($Address -match $pattern) {
        Write-Host "'$Address' is a valid email address"
    } else {
        Write-Host "'$Address' is NOT a valid email address"
    }
}

Test-EmailAddress "user@example.com"
Test-EmailAddress "not-an-email"
Test-EmailAddress "missing@tld."
```

Result:

```
'user@example.com' is a valid email address
'not-an-email' is NOT a valid email address
'missing@tld.' is NOT a valid email address
```

## Parsing structured log lines into objects

Combining regex with `[PSCustomObject]` lets you turn raw log data into structured objects that can be filtered, sorted, and exported.

```powershell
$logLines = @(
    "2024-06-19 08:01:44 INFO  Service started",
    "2024-06-19 08:15:22 ERROR Disk usage above 90%",
    "2024-06-19 08:17:05 WARN  Memory pressure detected",
    "2024-06-19 08:20:11 ERROR Connection timeout on port 443"
)

$pattern = '^(?<date>\d{4}-\d{2}-\d{2}) (?<time>\d{2}:\d{2}:\d{2}) (?<level>\w+)\s+(?<message>.+)$'

$entries = foreach ($line in $logLines) {
    if ($line -match $pattern) {
        [PSCustomObject]@{
            Date    = $Matches.date
            Time    = $Matches.time
            Level   = $Matches.level
            Message = $Matches.message
        }
    }
}

# Show only errors
$entries | Where-Object { $_.Level -eq "ERROR" } | Format-Table -AutoSize
```

Result:

```
Date       Time     Level Message
----       ----     ----- -------
2024-06-19 08:15:22 ERROR Disk usage above 90%
2024-06-19 08:20:11 ERROR Connection timeout on port 443
```
