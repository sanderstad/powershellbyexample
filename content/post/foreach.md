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

The equivalent of the above is:

```powershell
$list = @('a', 'b', 'c', 'd');

$list | ForEach-Object { Write-Host $_ }
}
```

The `ForEach-Object` command has more options than the basic `foreach` loop.
