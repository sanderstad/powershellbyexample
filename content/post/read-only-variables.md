---
title: Read-Only Variables
date: '2022-02-22'
categories:
  - Example
weight: 30
---

PowerShell supports constants and read-only variables.  
Read-only variables are variables that cannot be changed, in a regular way.  

You create a read-only variable with the `New-Variable` command with the parameter `-Option ReadOnly`.

```powershell
New-Variable -Name myVar -Value 1337 -Option ReadOnly
$myVar
```

Let's try and change it

```
$myVar = 31337
```

{{< error >}}
```
Cannot overwrite variable myVar because it is read-only or constant.
At line:1 char:1
+ $myVar = 31337
+ ~~~~~~~~~~~~~~
    + CategoryInfo          : WriteError: (myVar:String) [], SessionStateUnauthorizedAccessException
    + FullyQualifiedErrorId : VariableNotWritable
```
{{< /error >}}

To change the variable we need to use the `-Force` parameter:

```powershell
$myvar          # Should output 1337
New-Variable -Name myVar -Value 31337 -Option ReadOnly -Force
$myVar          # Should output 31337
```

In this case the value can be changed
