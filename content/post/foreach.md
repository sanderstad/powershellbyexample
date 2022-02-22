---
title: foreach
date: '2022-02-22'
categories:
  - Example
---

Something

```powershell
$files = Get-ChildItem
foreach ($file in $files) {
  Write-Output $file.Name
}
```