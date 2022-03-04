---
title: Errors
author: Sander Stad
date: '2022-03-04'
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

### SilentlyContinue

### Ignore

### Inquire

