---
title: Hashtables
author: Sander Stad
date: '2022-02-23'
categories:
  - Example
weight: 110
---

In the arrays example we saw how to create arrays. In this example we will see how to create hashtables.

A hashtable is a data structure, similar like an array, but with a hashtable each value is stored with a key. You can compare is to a key/value store database structure.

To declare a hastable we use the `@{}` syntax:

```powershell
$employees = @{}
```

This is different from the arrays where the `@()` is used.

## Adding items to a hashtable

To add an item to a hashtable we use the `Add` method:

```powershell
$employees = @{}

# Adding values using integers
$employees.Add(1, "John")
$employees.Add(2, "Mary")
$employees.Add(3, "Bob")
$employees.Add(4, "Sam")

$address = @{}

# Adding values using strings
$address.Add("John", "123 Main Street")
$address.Add("Mary", "456 North Street")
$address.Add("Bob", "789 West Street")
$address.Add("Sam", "321 South Street")

# Creating the hashtable in one go with values
$zipCodes = @{
    "John" = "12345"
    "Mary" = "54321"
    "Bob" = "98765"
    "Sam" = "32145"
}

```

## Accessing items from a hashtable

To access an item from a hashtable you use the `[key]` syntax:

```powershell
$employees[4]
$address["Mary"]
$zipCodes.Sam
```

Result:

```
Sam
456 North Street
32145
```

Notice that this looks similar like an array, but the key is used instead of the index.
The key can be an integer or a string if you like.
The example above also directly used the key to get the value in the hashtable.

## Loop through a hashtable

Accessing a hashtable in a loop is similar to the example you saw before.  
There are several ways we can loop through a hashtable.

```powershell
$employees.keys | Sort-Object $_ | ForEach-Object {
    Write-Host "Employee ID $($_) : $($employees[$_])"
}

foreach ($key in $address.Keys) {
    Write-Host "$($key) lives at $($address[$key])"
}
```

Result:

```
Employee ID 1 : John
Employee ID 2 : Mary
Employee ID 3 : Bob
Employee ID 4 : Sam

Bob lives at 789 West Street
Mary lives at 456 North Street
John lives at 123 Main Street
Sam lives at 321 South Street
```

Notice that we can use the `keys` method to get the keys of the hashtable and then use that particular value in the loops to get the value.
The script also uses the `Sort-Object` command to sort the keys.

## Hastable with different properties

Within a hashtable you are not bound to the key/value pair.  
You can create a hashtable with different properties.

```powershell
$employeeAddress = @{
    Name    = "Mary"
    Address = "456 North Street"
    Zipcode = "54321"
}

$employeeAddress
```

```
Name                           Value
----                           -----
Name                           Mary
Zipcode                        54321
Address                        456 North Street
```

## Removing properties from a hashtable

To remove a property from a hashtable you use the `Remove` method:

```powershell
$employeeAddress = @{
    Name    = "Mary"
    Address = "456 North Street"
    Zipcode = "54321"
}

$employeeAddress.Remove("Zipcode")

$employeeAddress
```

Result:

```
Name                           Value
----                           -----
Name                           Mary
Address                        456 North Street
```

## Sort a hashtable

There are various ways we can sort a hashtable.

The first thing we can do is create an ordered hashtable. This will result in the keys to be sorted.

```powershell
$addresses = @()

$addresses += [ordered]@{Name = "John"; Address = "123 Main Street" }
$addresses += [ordered]@{Name = "Sam"; Address = "321 South Street" }

$addresses += @{Name = "Mary"; Address = "456 North Street" }
$addresses += @{Name = "Bob"; Address = "789 West Street" }

$addresses
```

Result:

```
Name                           Value
----                           -----
Name                           Value
----                           -----
Name                           John
Address                        123 Main Street
Name                           Sam
Address                        321 South Street

Address                        456 North Street
Name                           Mary
Address                        789 West Street
Name                           Bob
```

What happens here is that the first and last item keys are ordered differently. In the first two rows the `Name` and `Address` keys are displayed and for the next two rows the `Address` and `Name` keys are displayed.  
That is because it is possible to set the order in which the items need to be iterated through. Once the `ordered` keyword is used the items will be displayed in that order.

Another thing we can do is sort the hashtables by key:

```powershell
$addresses = @()

$addresses += @{Name = "John"; Address = "123 Main Street" }
$addresses += @{
  Name = "Sam"
  Address = "321 South Street" 
}
$addresses += @{Name = "Mary"; Address = "456 North Street" }
$addresses += @{Name = "Bob"; Address = "789 West Street" }

$addresses | Sort-Object -Property @{e = { $_.Name } }
```

Result:

```
Name                           Value
----                           -----
Address                        789 West Street
Name                           Bob
Address                        123 Main Street
Name                           John
Address                        456 North Street
Name                           Mary
Address                        321 South Street
Name                           Sam
```
