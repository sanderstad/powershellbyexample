---
title: Foreach
date: '2022-02-22'
categories:
  - Example
weight: 60
---

Foreach is a loop that iterates over a collection.

```powershell
$list = @('a', 'b', 'c', 'd');

foreach($item in $list){
    Write-Host $item
}
```

Result:

```
a
b
c
d
```

The equivalent of the above is:

```powershell
$list = @('a', 'b', 'c', 'd');

$list | ForEach-Object { Write-Host $_ }
```

Result:

```
a
b
c
d
```

The `ForEach-Object` command has more options than the basic `foreach` loop. For more info about the `ForEach-Object` command run the `Get-Help` command.

```powershell
Get-Help ForEach-Object
```
