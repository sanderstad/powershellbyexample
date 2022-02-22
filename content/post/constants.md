---
title: Constants
date: '2022-02-22'
categories:
  - Example
weight: 40
---

PowerShell supports constants and read-only variables.  
Constants are variables that cannot be changed whatever you try.

You create a constant variable with the `New-Variable` command with the parameter `-Option Constant`.

```powershell
New-Variable -Name myConst -Value "This CANNOT be changed" -Option Constant
$myConst
```

We can't change te value even with the `-Force` parameter

```powershell
New-Variable -Name myConst -Value "I'm going to change it" -Option Constant -Force
```

{{< error >}}
```
New-Variable : Cannot overwrite variable myConst because it is read-only or constant.
At line:1 char:1
+ New-Variable -Name myConst -Value "I'm going to change it" -Option Co ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : WriteError: (myConst:String) [New-Variable], SessionStateUnauthorizedAccessException
    + FullyQualifiedErrorId : VariableNotWritable,Microsoft.PowerShell.Commands.NewVariableCommand
```
{{< /error >}}

