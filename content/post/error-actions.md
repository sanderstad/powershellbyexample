---
title: Error Actions
author: Sander Stad
date: '2022-03-09'
categories:
  - Example
weight: 150
---

In normal circumstances, because we want PowerShell to work even though sometimes we have errors.
That is the reason why PowerShell errors in general are what we call non-terminating errors.

```powershell
Clear-Host

$items = @()

# Generate more items
$items += for ($i = 0; $i -le 3; $i++) {
    "$($env:TEMP)\$(Get-Process -Id $pid)-$($i).txt"
}

# Let's generate some errors
$items | ForEach-Object {
    Get-Item -Path $_
}
```

Result:

{{< error >}}
```
Get-Item : Cannot find path 'C:\Users\Sander\AppData\Local\Temp\System.Diagnostics.Process (powershell)-0.txt' because it does not exist.
At line:2 char:5
+     Get-Item -Path $_
+     ~~~~~~~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (C:\Users\Sander...wershell)-0.txt:String) [Get-Item], ItemNotFoundException
    + FullyQualifiedErrorId : PathNotFound,Microsoft.PowerShell.Commands.GetItemCommand

Get-Item : Cannot find path 'C:\Users\Sander\AppData\Local\Temp\System.Diagnostics.Process (powershell)-1.txt' because it does not exist.
At line:2 char:5
+     Get-Item -Path $_
+     ~~~~~~~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (C:\Users\Sander...wershell)-1.txt:String) [Get-Item], ItemNotFoundException
    + FullyQualifiedErrorId : PathNotFound,Microsoft.PowerShell.Commands.GetItemCommand

Get-Item : Cannot find path 'C:\Users\Sander\AppData\Local\Temp\System.Diagnostics.Process (powershell)-2.txt' because it does not exist.
At line:2 char:5
+     Get-Item -Path $_
+     ~~~~~~~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (C:\Users\Sander...wershell)-2.txt:String) [Get-Item], ItemNotFoundException
    + FullyQualifiedErrorId : PathNotFound,Microsoft.PowerShell.Commands.GetItemCommand

Get-Item : Cannot find path 'C:\Users\Sander\AppData\Local\Temp\System.Diagnostics.Process (powershell)-3.txt' because it does not exist.
At line:2 char:5
+     Get-Item -Path $_
+     ~~~~~~~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (C:\Users\Sander...wershell)-3.txt:String) [Get-Item], ItemNotFoundException
    + FullyQualifiedErrorId : PathNotFound,Microsoft.PowerShell.Commands.GetItemCommand
```
{{< /error >}}

Although we got a lot of errors, PowerShell still executes the script

## Error Actions

There are six error actions in PowerShell
* Stop
* Continue
* SilentlyContinue
* Ignore
* Inquire
* Suspend (Only used for Windows PowerShell Workflow. Cannot be used with advanced functions)

### Stop

Let's run the same script as before but now with an error action called `Stop`.

```powershell
Clear-Host

$items = @()

# Generate more items
$items += for ($i = 0; $i -le 3; $i++) {
    "$($env:TEMP)\$(Get-Process -Id $pid)-$($i).txt"
}

# Let's generate some errors
$items | ForEach-Object {
    Get-Item -Path $_ -ErrorAction Stop
}
```

Result:

{{< error >}}
```
Get-Item : Cannot find path 'C:\Users\Sander\AppData\Local\Temp\System.Diagnostics.Process (powershell)-0.txt' because it does not exist.
At line:2 char:5
+     Get-Item -Path $_ -ErrorAction Stop
+     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (C:\Users\Sander...wershell)-0.txt:String) [Get-Item], ItemNotFoundException
    + FullyQualifiedErrorId : PathNotFound,Microsoft.PowerShell.Commands.GetItemCommand
```
{{< /error >}}

Although in the previous example we could loop through the items, this time because we set an error action, the loop stops at the first error.

### Continue

This is the default error action. The error will still be displayed but the script will continue.

```powershell
Clear-Host

$items = @()

# Generate more items
$items += for ($i = 0; $i -le 3; $i++) {
    "$($env:TEMP)\$(Get-Process -Id $pid)-$($i).txt"
}

# Let's generate some errors
$items | ForEach-Object {
    Get-Item -Path $_ -ErrorAction Continue
}
```

Result:

{{< error >}}
```
Get-Item:
Line |
   2 |      Get-Item -Path $_ -ErrorAction Continue
     |      ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     | Cannot find path 'C:\Users\Sander\AppData\Local\Temp\System.Diagnostics.Process (pwsh)-0.txt' because it does not exist.
Get-Item:
Line |
   2 |      Get-Item -Path $_ -ErrorAction Continue
     |      ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     | Cannot find path 'C:\Users\Sander\AppData\Local\Temp\System.Diagnostics.Process (pwsh)-1.txt' because it does not exist.
Get-Item:
Line |
   2 |      Get-Item -Path $_ -ErrorAction Continue
     |      ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     | Cannot find path 'C:\Users\Sander\AppData\Local\Temp\System.Diagnostics.Process (pwsh)-2.txt' because it does not exist.
Get-Item:
Line |
   2 |      Get-Item -Path $_ -ErrorAction Continue
     |      ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     | Cannot find path 'C:\Users\Sander\AppData\Local\Temp\System.Diagnostics.Process (pwsh)-3.txt' because it does not exist.
```
{{< /error >}}

### SilentlyContinue

With SilentlyCOntinue the error message will not be displayed and the script executes the pipeline commands.

```powershell
Clear-Host

$items = @()

# Generate more items
$items += for ($i = 0; $i -le 3; $i++) {
    "$($env:TEMP)\$(Get-Process -Id $pid)-$($i).txt"
}

# Let's generate some errors
$items | ForEach-Object {
    Get-Item -Path $_ -ErrorAction SilentlyContinue
}

$error[0]
```

No error should be displayed, but the error is still saved in the `$Error` variable.

Result:

```
Get-Item:
Line |
   2 |      Get-Item -Path $_ -ErrorAction SilentlyContinue
     |      ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
     | Cannot find path 'C:\Users\Sander\AppData\Local\Temp\System.Diagnostics.Process (pwsh)-3.txt' because it does not exist.
```

### Ignore

The `Ignore` action is the same as the Silentlycontinue except the error output is *not* stored into `$Error` variable.

```powershell
Clear-Host

$items = @()

# Generate more items
$items += for ($i = 0; $i -le 3; $i++) {
    "$($env:TEMP)\$(Get-Process -Id $pid)-$($i).txt"
}

# Let's generate some errors
$items | ForEach-Object {
    Get-Item -Path $_ -ErrorAction Ignore
}

$error[0]
```

THe result should be that the script returns nothing at all.

### Inquire

With `Inquire` PowerShellhelps when an error occurred due to cmdlet. It will give the option to the user with choices and prompt for appropriate action.

```powershell
Clear-Host

$items = @()

# Generate more items
$items += for ($i = 0; $i -le 3; $i++) {
    "$($env:TEMP)\$(Get-Process -Id $pid)-$($i).txt"
}

# Let's generate some errors
$items | ForEach-Object {
    Get-Item -Path $_ -ErrorAction Inquire
}
```

Result:

```
Confirm
Cannot find path 'C:\Users\Sander\AppData\Local\Temp\System.Diagnostics.Process (pwsh)-0.txt' because it does not exist.
[Y] Yes  [A] Yes to All  [H] Halt Command  [S] Suspend  [?] Help (default is "Y"):
```