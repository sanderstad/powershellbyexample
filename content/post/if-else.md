---
title: If/Else
author: Sander Stad
date: '2022-02-23'
categories:
  - Example
weight: 70
---

Using `if` and `else` statements in PowerShell is pretty straightforward.

You can have an `if` statement without an `else` statement.

```powershell
$value = 5

if($value > 1){
    Write-Host "value is greater than 1"
}
```

You can have an `if` statement with a single `else` statement, or an `if` statement with an `else if` statement.

```powershell
$value = 5

if ($value -gt 10) {
    Write-Host "value is greater than 10"
}
else {
    Write-Host "value is $value"
}

if ($value -gt 10) {
    Write-Host "value is greater than 10"
}
elseif ($value -lt 10) {
    Write-Host "value is less than 10"
}
else {
    Write-Host "value is 10"
}
```
