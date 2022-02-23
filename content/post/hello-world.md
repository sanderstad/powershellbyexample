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

As you can see the `Write-Host` outputs the value to the console while the `Write-Output` command stores the output in the variable.



