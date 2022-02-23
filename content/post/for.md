---
title: For
date: '2022-02-22'
categories:
  - Example
weight: 50
---

Classic for loop

```powershell
for ($i = 1; $i -le 5; $i++){
    Write-Host $i
}
```

Result:

```
1
2
3
4
5
```

We can also use text in our for loop conditions

```powershell
for ($i = '' ; $i.length -le 20; $i += '='){
    Write-Host $i
}
```

Result:

```
=
==
===
====
=====
======
=======
========
=========
==========
===========
============
...
...
====================
```