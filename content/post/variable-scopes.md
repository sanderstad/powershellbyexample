---
title: Scopes
author: Sander Stad
date: '2022-02-22'
categories:
  - Example
weight: 130
---

When you create a variable, alias or a function in PowerShell, it is only available in the current scope where it was created.  
For example, when you create a variable in a function, it is only available in the function.  
When you create a variable in a script, it is available in the script and all functions in the script.

There is a way to these items available outside it's current scope.

## Local 

The local scope is relative to whatever context the code runs in at the time.

To define an item local you use the *local:* modifier. By default the scope of a variable is the _Local_ scope.

```powershell
$var = "bla"

$var
```

Result:

```
bla
```

## Global 

Items in the global scope are available everywhere.

To define an item as global you use the *global:* modifier.

```powershell
$global:varOne = "bla"                              # Assign a variable in the global scope

Write-Host "Variable One:" $global:varOne           # Print the variable

# Function to demonstrate local and global scope
function MyFunc() {
    $global:varOne = "bla bla"
    $varTwo = "boo"
    return $varTwo
}

Write-Host "Variable Two:" $varTwo                  # Print the variable

$varTwo = MyFunc                                    # Call the function and change the variable
Write-Host "Variable One:" $varOne                  # Print the variable

Write-Host "Variable Two:" $varTwo                  # Print the variable
```

Result:

```
Variable One: bla
Variable Two:
Variable One: bla bla
Variable Two: boo
```

As you can see the global variable we defined a global variable called *varOne* and we changed the value of *varOne* in the function.
We did not have access to variable *varTwo* because the scope of the variable was set to local in the function.

## Private 

Items that are defined as private are not available outside the scope where they are defined.

To define an item private you use the *private:* modifier.

```powershell
# Make sure that the variable is gone
Remove-Variable -Name var1 -ErrorAction SilentlyContinue

$var1 = "This is a variable"

Write-Host "var1 = '$var1'"

function test1 {
    Write-Host "Inside function, var1 = $var1"
}

test1

# Now let's do it privately
Remove-Variable -Name var1 -ErrorAction SilentlyContinue

$Private:var1 = "This is a variable"

function test2 {
    Write-Host "Inside function with private, var1 = $var1"
}

test2
```

Result

```
var1 = 'This is a variable'
Inside function, var1 = This is a variable
Inside function with private, var1 = 
```

As you can see the function *test2* did not have access to the variable *var1* because it was defined as private.

## Script 

A script scope is automatically created every time a PowerShell script runs.

To define an item script you use the *script:* modifier.

```powershell
function myFunc {

    $Script:VarOne = "Script Scoped"
    $Var2 = "Function Scoped"
}

myFunc
Write-Host "Var 1: $VarOne"
Write-Host "Var 2: $Var2"
```

Result:

```
Var 1: Script Scoped
Var 2: 
```

As you can see the script is not able to access the variables `$Var2` but is able to access the variable `$VarOne`.


