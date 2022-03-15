---
title: Arrays
author: Sander Stad
date: '2022-02-23'
categories:
  - Example
weight: 90
---

An array is a data structure that is designed to store a collection of items.
The items can be the same type or different types.

## Creating arrays

Define an array using the `@()`

```powershell
$values = @("One", "Two", "Three", "Four", "Five")
$values
$values.GetType()
```

Result:

```
One
Two
Three
Four
Five

IsPublic IsSerial Name                                     BaseType
-------- -------- ----                                     --------
True     True     Object[]                                 System.Array
```

Define an array by setting with comma separated values

```powershell
$values = "Six", "Seven", "Eight", "Nine", "10"
$values
```

Result:

```
Six
Seven
Eight
Nine
10
```

Define an array with a specific type

```powershell
[int[]]$values = 6, 7, 8, 9, 10
$values
```

Result:

```
6
7
8
9
10
```

Define an array with the [array] type

```powershell
[array]$values = 11, 12, 13, 14, 15
$values
```

Result:

```
11
12
13
14
15
```

## Adding and changing values in arrays

```powershell
$values = @("One", "Two", "Three")
$values

# Counting the items in the array using the Count property
Write-Host "Items in array $($values.Count)"

# Add a value to the array using the + operator
$values += "Four"
$values

Write-Host "Items in array $($values.Count)"

# Change a value in the array using the index
$values[0] = "Five"
$values
```

Result:

```
One
Two
Three
Items in array 3
One
Two
Three
Four
Items in array 4
Five
Two
Three
Four
```

## Accessing arrays

To access the items in the array, use the index.
The index starts at 0.

```powershell
[array]$values = 1, 2, 3, 4, 5

# Access the third item in the array
Write-Host "Item at index 2: $($values[2])"
```

Result:

```
Item at index 2: 3
```

## Looping though arrays

The same index method as the example before can be used with loops.

```powershell
$nameArray = @("Erik", "Penny", "Randy", "Sandy", "Toby", "Uma", "Vicky", "Will", "Xavier", "Yvette", "Zach")
for ($i = 0; $i -lt $nameArray.Length; $i++) {
    Write-Host $nameArray[$i]
}
```

Result:

```
Erik
Penny
Randy
Sandy
Toby
Uma
Vicky
Will
Xavier
Yvette
Zach
```

