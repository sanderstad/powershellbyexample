---
title: Splatting
author: Sander Stad
date: '2022-04-11'
categories:
  - Example
weight: 220
---

In some situations we have so many parameters that we cannot fit it on a single line or have to use the "`" sign to separate lines.  
I'm not a fan of having very long lines or the backticks because it makes the code hard to read.  
Instead we can use variable splatting to pass multiple parameters to a function.

A splatted variable is a collection of values that is passed to a function as a single parameter.  
This type of variable is in essence a hashtable.

In the example below we will use the `Get-ChildItem` command to get a list of all the files in a directory.

```powershell
Get-ChildItem -Path $env:TEMP -Include "*.txt" -Depth 2 -Recurse
```

This line is not to long yet but you can imagine that some functions/commands have a lot more parameters.

We can rewrite the same command using variable splatting.

```powershell
$params = @{
    Path = $env:TEMP
    Include = "*.txt"
    Depth = 2
    Recurse = $true
}
Get-ChildItem @params
```

As you can see this is a lot more readable and easier to understand.