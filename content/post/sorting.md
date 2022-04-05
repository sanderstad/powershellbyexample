---
title: Sorting
author: Sander Stad
date: '2022-04-05'
categories:
  - Example
weight: 200
---

Sorting values is important in any programming language. To do this in PowerShell you can use the `Sort-Object` command. By default the sort is done in ascending order and case insensitive.

Here are some examples how to sort data in PowerShell.

## Sort by value

```powershell
$names = @("Muffin","Romeo","Noodle","Zoe","Jack","Luna","Gracie","mittens","Phoebe","Peanut","Harley","Jake")

$names | Sort-Object
```

The result:

```
Gracie
Harley
Jack
Jake
Luna
mittens
Muffin
Noodle
Peanut
Phoebe
Romeo
Zoe
```

## Sort by descending order

By default any data is sorted in ascending order. To sort in descending order you can use the `-Descending` parameter.

```powershell
$names = @("Muffin","Romeo","Noodle","Zoe","Jack","Luna","Gracie","mittens","Phoebe","Peanut","Harley","Jake")

$names | Sort-Object -Descending
```

The result:

```
Zoe
Romeo
Phoebe
Peanut
Noodle
Muffin
mittens
Luna
Jake
Jack
Harley
Gracie
```

The same can be done with a `-Ascending` parameter to sort in ascending order.  

## Sort by case sensitivity

By default the sort is done in case insensitive order, but you are able to change this with the `-CaseSensitive` parameter.

```powershell
$names = @("Muffin","muffin","Noodle","Zoe","zoe","Luna","Gracie","peanut","Phoebe","Peanut","Harley","Jake")

$names | Sort-Object -CaseSensitive
```

The result:

```
Gracie
Harley
Jake
Luna
muffin
Muffin
Noodle
peanut
Peanut
Phoebe
zoe
Zoe
```

## Sort by unique values

You are able to sort the object in an array and remove all duplicates in the process using the `-Unique` parameter.

```powershell
$names = @("Muffin","muffin","Noodle","Zoe","zoe","Luna","Gracie","peanut","Phoebe","Peanut","Harley","Jake")

$names | Sort-Object -Unique
```

The result is:

```
Gracie
Harley
Jake
Luna
muffin
Noodle
Peanut
Phoebe
zoe
```

The values are sorted in an ascending order and case insensitive.

## Sort by property

If an object in an array has any properties, you are able to sort by those properties using the `-Property` parameter.

```powershell
$files = Get-ChildItem -Path $env:TEMP
$files | Sort-Object -Property LastWriteTime | Where-Object{$_.PSIsContainer -eq $false}
```

The result would look something like this:

```

    Directory: C:\Users\Sander\AppData\Local\Temp


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        1/30/2020   1:30 PM            999   nsd8F8A.tmp
-a----        11/5/2020   9:52 AM         339816   Chipset.dll
-a----        1/13/2022   9:57 AM          15021   nscA6CC.tmp
-a----        3/25/2022   8:12 PM            942   wctBA18.tmp
-a----        3/25/2022   8:12 PM            942   wct5787.tmp
-a----        3/25/2022   8:12 PM            942   wctEF84.tmp
-a----        3/28/2022   9:21 AM        3192224   94e81122-594d-4910-b16b-3c9d9b28bff1.tmp
-a----        3/28/2022   7:49 PM           1825   wct3C2A.tmp
-a----        3/28/2022   7:49 PM           1825   wct9D9A.tmp
-a----        3/29/2022  10:15 AM              0 况  mat-debug-35588.log
-a----        3/29/2022  10:16 AM              0 况  mat-debug-26716.log
-a----        3/29/2022  10:22 AM              0 况  mat-debug-20228.log
-a----        3/29/2022  11:22 AM              0 况  mat-debug-15472.log
-a----        3/29/2022  12:22 PM              0 况  mat-debug-18972.log
-a----        3/29/2022   1:22 PM              0 况  mat-debug-7244.log
-a----        3/29/2022   1:22 PM              0 况  mat-debug-33388.log
-a----        3/29/2022   2:22 PM              0 况  mat-debug-28752.log
-a----        3/29/2022   2:25 PM          57833   wct2B1F.tmp
```