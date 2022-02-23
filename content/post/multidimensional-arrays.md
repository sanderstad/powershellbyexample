---
title: Multidimensional Arrays
author: Sander Stad
date: '2022-02-23'
categories:
  - Example
weight: 100
---

There are two types of multidimensional arrays, jagged and true multidimensional arrays.

## Jagged arrays

Jagged arrays are the arrays you will probably use the most. These types of arrays are very cost effective because the dimensions can be different in size.

```powershell
$array = @(1, 2, (1, 2, 3), 3, 4, (10, 11, 12), 5)
$array[0]
$array[1]
$array[2]
$array[2][0]
$array[2][1]
$array[5]
```

Result:

```
1
2
1
2
3
1
2
10
11
12
```

## True multidimensional arrays

With true multidimensional arrays can be compared to a matrix. You define the size of the array/matrix when you create it.

```powershell
$array = New-Object 'object[,]' 5,8
$array[2,5] = 'Hello'
$array[3,7] = 'World!'
$array
```

Result:

```
Hello
World!
```

You can compare the above with the following:

```
[ ][ ][ ]    [ ]    [ ]
[ ][ ][ ]    [ ]    [ ]
[ ][ ][ ]    [ ]    [ ]
[ ][ ][ ]    [ ]    [ ]
[ ][ ][ ]    [ ]    [ ]
[ ][ ][Hello][ ]    [ ]
[ ][ ][ ]    [ ]    [ ]
[ ][ ][ ]    [World][ ]
```




