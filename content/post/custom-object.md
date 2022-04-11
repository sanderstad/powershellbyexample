---
title: Custom Objects
author: Sander Stad
date: '2022-04-11'
categories:
  - Example
weight: 210
---

Custom objects are a powerful feature of PowerShell and can be leveraged to make your function/commands even more suitable for advanced use cases.
It is an easy way to create structured data without any fuzz. Importing and exporting data will also be muc easier.

To create a custom object we can to use the `New-Object` command or use the [PSCustomObject] type.

# Creating a custom object

```powershell
# Old style of creating an object
$object1 = New-Object PSObject

Add-Member -InputObject $object1 -MemberType NoteProperty -Name prop1 -Value "value1"
Add-Member -InputObject $object1 -MemberType NoteProperty -Name prop2 -Value "value2"

$object1
$object1.GetType()
```

The result should look something like this:

```
prop1  prop2
-----  -----
value1 value2

IsPublic IsSerial Name                                     BaseType
-------- -------- ----                                     --------
True     False    PSCustomObject                           System.Object
```

Of course we are going to use the new style of creating an object.

```powershell
# New style of creating an object
$object2 = [PSCustomObject]@{
    prop1 = "value1"
    prop2 = "value2"
}

$object2
```

The result should look something like this:

```
prop1  prop2
-----  -----
value1 value2

IsPublic IsSerial Name                                     BaseType
-------- -------- ----                                     --------
True     False    PSCustomObject                           System.Object
```

As you can see the results are exactly the same but it is a lot easier to write it the new way.
