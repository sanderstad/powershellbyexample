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

The `Out-File` command is easiest used in combination with a pipe ("|").

```powershell
"Lorem ipsum dolor sit amet, consectetur adipiscing elit" | Out-File -FilePath c:\temp\output1.txt
Get-Content C:\temp\output1.txt
```

Result:

```
Lorem ipsum dolor sit amet, consectetur adipiscing elit
```

Without any other parameter `Out-File` will overwrite the file. We can use the `-Append` parameter to append text.

```powershell
"Lorem ipsum dolor sit amet, consectetur adipiscing elit" | Out-File -FilePath c:\temp\output2.txt
"Ut enim ad minim veniam, quis nostrud" | Out-File -FilePath c:\temp\output2.txt -Append
Get-Content C:\temp\output2.txt
```

Result:

```
Lorem ipsum dolor sit amet, consectetur adipiscing elit
Ut enim ad minim veniam, quis nostrud
```

