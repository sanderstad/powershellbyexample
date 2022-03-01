---
title: Strings
author: Sander Stad
date: '2022-03-01'
categories:
  - Example
weight: 140
---


# Defining strings

There are various ways you define a string in PpowerShell


```powershell
$var = "Hello World!"

"Lorem ipsum dolor sit amet..."
$value1 = "Ut enim ad minim veniam... $var"
$value2 = 'Duis aute irure dolor in... $var'
[string]$value3 = "Excepteur sint occaecat cupidatat non proident..."

$value1, $value2, $value3
write-host $value1, $value2, $value3
write-host $value1 $value2 $value3
```

Result:

```
Lorem ipsum dolor sit amet...
Ut enim ad minim veniam... Hello World!
Duis aute irure dolor in... $var
Excepteur sint occaecat cupidatat non proident...
Ut enim ad minim veniam... Hello World! Duis aute irure dolor in... $var Excepteur sint occaecat cupidatat non proident...
Ut enim ad minim veniam... Hello World! Duis aute irure dolor in... $var Excepteur sint occaecat cupidatat non proident...
```

In the third line in the example above, the string is written to screen because we haven't given PowerShell anything to do with it.  

In the fourth line, we assign the string to a variable using double quotes. This is just a string. Although this is a string, using double quotes we are able to use variables in the string.  

In the fifth line, we assign the string to a variable using single quotes. Again this is just a string. Because we are using single quotes, the string in the variable is taken literally and no variables can be included like in the fourth line.  

In the fifth line, we assign the string to a variable using the PowerShell notation. This is also string.

In the remaining lines we print out the variables in a couple different ways. 

# Concatenating Strings


# Joining Strings


# Splitting Strings


# Formatting Strings


# Strings in Objects





