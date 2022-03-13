---
title: Writing Files
author: Sander Stad
date: '2022-03-10'
categories:
  - Example
weight: 180
---

There are several methods to write data to files

## Add-Content

To add content to a file run the following script:

```powershell
Add-Content -Path (Join-Path -Path $env:TEMP -ChildPath "test1.txt") -Value "This is just a test"
Add-Content -Path (Join-Path -Path $env:TEMP -ChildPath "test1.txt") -Value "This is just another test"
Get-Content (Join-Path -Path $env:TEMP -ChildPath "test1.txt")
```

Result:

```
This is just a test
This is just another test
```

In this example you see that the command `Add-Content` automatically creates the file if it doesn't exist.
It also adds the value to another line.

What if you don't want to add a new line

```powershell
Add-Content -Path (Join-Path -Path $env:TEMP -ChildPath "test2.txt") -Value "Test1"
Add-Content -Path (Join-Path -Path $env:TEMP -ChildPath "test2.txt") -Value "Test2" -NoNewline
Add-Content -Path (Join-Path -Path $env:TEMP -ChildPath "test2.txt") -Value "Test3" -NoNewline
```

Result:

```
Test1
Test2Test3
```

## Out-File



## Write-Output



