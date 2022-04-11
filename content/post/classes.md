---
title: Classes
author: Sander Stad
date: '2022-04-11'
categories:
  - Example
weight: 230
---

Since PowerShell 5 you are able to create your own classes in PowerShell.

Although classes in other programming languages are a big advantage and sometimes even required, they are not required in PowerShell.

In some cases you may want to use classes in PowerShell to create objects that can be used in PowerShell. One of those examples is when you use DSC (Desired State Configuration).
Most of the time you will not need to use classes in PowerShell, and you will be fine using custom objects in your commands instead.

# Create a class

To create a class we need to use the `class` keyword. The example below shows an example of a class.  
It also shows how to use the `new` keyword to create an instance of the class.

```powershell
class Tree {
    [int]$Height
    [int]$Age
    [string]$Color
}

$tree1 = new-object Tree
$tree2 = [Tree]::new()

$tree1.Height = 10
$tree1.Age = 5
$tree1.Color = "Red"

$tree2.Height = 20
$tree2.Age = 10
$tree2.Color = "Green"

$tree1
$tree2
```

The result should look something like this:

```
------ --- -----
    10   5 Red
    20  10 Green
```

As you can see we can use the `New-Object` command or the `new()` keyword to create an instance of the class.

# Constructors

When creating a class you can also create a constructor. The constructor is a special method that is called when you create an instance of the class.  
This can be very useful when you want to initialize some properties of the class.

The example below demonstrates how to create multiple constructors for the class.

```powershell
class Tree {
    [int]$Height
    [int]$Age
    [string]$Color

    Tree() {
        $this.Height = 1
        $this.Age = 0
        $this.Color = "Green"
    }

    Tree([int]$Height, [int]$Age, [string]$Color) {
        $this.Height = $Height;
        $this.Age = $Age;
        $this.Color = $Color;
    }
}

$tree1 = [Tree]::New()
$tree2 = New-Object Tree 5, 2, "Red"

$tree1
$tree2
```
The result should look something like this:

```
Height Age Color
------ --- -----
     1   0 Green
     5   2 Red
```

In the example above you can see the first object is created with the default constructor and does not accept any parameters. This can be handy to initialize variables when the object is created.
The second object is created using another constructor and accepts three parameters.

# Creating methods

We technically don't need to create methods to set values in the class because all the properties can be retrieved and set using the default methods.  
In some situations you may want to create methods to set values in the class.

In the example below we create a method to let the tree grow by increasing the age and the height.

```powershell
class Tree {
    [int]$Height
    [int]$Age
    [string]$Color

    # Initialize the tree by setting default values
    Tree() {
        $this.Height = 1
        $this.Age = 0
        $this.Color = "Green"
    }

    # Create a constructor with parameters a.k.a. constructor overloading
    Tree([int]$Height, [int]$Age, [string]$Color) {
        $this.Height = $Height
        $this.Age = $Age
        $this.Color = $Color
    }

    [void]Grow() {
        # Get a random height because plants and trees don't grow the same each year
        $heightIncrease = Get-Random -Min 1 -Max 5;
        $this.Height += $heightIncrease;
        $this.Age += 1
    }
}

$tree = [Tree]::New()

# Let the tree grow for 10 years
for ($i = 0; $i -lt 10; $i++) {
    $tree.Grow()
    $tree
}
```

The result should look something like this:

```
Height Age Color
------ --- -----
     3   1 Green
     7   2 Green
    10   3 Green
    11   4 Green
    15   5 Green
    16   6 Green
    20   7 Green
    24   8 Green
    26   9 Green
    28  10 Green
```

# Class inheritance

The examples are all good to use with classes but can be done very easily without using classes.  
Where classes become useful is when you start to use inheritance.

```powershell
class Tree {
    [int]$Height
    [int]$Age
    [string]$Color

    Tree() {
        $this.Height = 1;
        $this.Age = 0
        $this.Color = "Green"
    }

    Tree([int]$Height, [int]$Age, [string]$Color) {
        $this.Height = $Height;
        $this.Age = $Age;
        $this.Color = $Color;
    }

    [void]Grow() {
        # Get a random height because plants and trees don't grow the same each year
        $heightIncrease = Get-Random -Min 1 -Max 5;
        $this.Height += $heightIncrease;
        $this.Age += 1
    }
}

class AppleTree : Tree {
    [string]$Spieces = "Apple"
}

$tree = [AppleTree]::new()
$tree
```

The result should look something like this:

```
Spieces Height Age Color
------- ------ --- -----
Apple        1   0 Green
```

For more information about classes in PowerShell see [Classes](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_classes?view=powershell-7.2).



