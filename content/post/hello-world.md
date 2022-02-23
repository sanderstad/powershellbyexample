---
title: Hello World
date: '2022-02-22'
categories:
  - Example
weight: 10
---

As with every first program we write, we will print the classic "hello world" message. Here’s the full source code.

```powershell
Write-Host 'Hello, World!'
'Hello, World!' | Write-Host
```

Result:

```
Hello, World!
Hello, World!
```

There is another method to print the output or the strings to screen and that is is `Write-Output`.

```powershell
Write-Output 'Hello, World!'
'Hello, World!' | Write-Output
```

This will print the exact same thing as the previous snippet.

Result:

```
Hello, World!
Hello, World!
```

It all becomes different when the two commands are used to store the output.

```powershell
$wh = 'Hello, World!' | Write-Host
$wo = 'Hello, World!' | Write-Output

Get-Variable wh

Get-Variable wo
```

Result:

```
Name                           Value
----                           -----
wh
wo                             Hello, World!
```

https://www.tutorialspoint.com/difference-between-write-output-and-write-host-command-in-powershell



