---
title: Variables
date: '2022-02-22'
categories:
  - Example
weight: 20
---

In PowerShell we can store all types of values in PowerShell variables. It's possible to store the results of commands, and store elements that are used in commands and expressions, such as names, paths, settings, and values.

Variable unlike other languages are note case-sensitive, and can include spaces and special characters. Variable names with special characters and spaces are difficult to use and should be avoided. For more information, see [Variable names that include special characters](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_variables?view=powershell-7.2#variable-names-that-include-special-characters).

PowerShell automatically detects the type of the variable and converts it to the appropriate type. It is also possible to declare variables with a specific type.

```powershell
$a = 1337                               # System.Int32
$b = "Swifty"                           # System.String
$c = 31337, "Swifty"                    # array of System.Int32, System.String
$d = Get-ChildItem C:\Windows           # FileInfo and DirectoryInfo types
New-Variable -Name e -Value 1337        # System.Int32
```

PowerShell is even smart enough to detect when a variable is assigned a value that is not of the same type and convert it to the appropriate type.

```powershell
$number = "1337"   # The string is converted to an integer.
$number.GetType()   # Get the type of the variable.
```

As soon as the variable is assigned a value, it is considered to be of that data type. This means that it cannot be changed without converting it to a different type.

```powershell
$number = 1337
$number = "One Thousand, Three Hundred and Thirty-Seven"  #This will give an error
```

{{< error >}}
```
Cannot convert value "One Thousand, Three Hundred and Thirty-Seven" to type "System.Int32". Error: "Input string was not in a correct format."
At line:1 char:1
+ $number = "One Thousand, Three Hundred and Thirty-Seven"  #This will  ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : MetadataError: (:) [], ArgumentTransformationMetadataException
    + FullyQualifiedErrorId : RuntimeException
```
{{< /error >}}