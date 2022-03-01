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

There are various ways you can concatenate strings in PowerShell.

```powershell
$firstName = "John"
$middleName = "Hubert"
$lastName = "Doe"

$fullName = $firstName + ' ' + $middleName + ' ' + $lastName
$fullName

$fullName = "$firstName $middleName $lastName"
$fullName
```

Result:

```
John Hubert Doe
John Hubert Doe
```

As you can see PowerShell is very flexible how you can concatenate strings.

# Joining Strings

We can use the `-join` operator to join multiple items together. We can assign a delimiter to the operator to define what separates the items.

```powershell
# Join array
$list = @('a', 'b', 'c', 'd', 'e')
$list -join ','

# Join array without declaring variable
'f', 'g', 'h', 'i', 'j' -join '-'

# Join array with separator
-join ('k', 'l', 'm', 'n', 'o')

# Using .Net string.Join
[string]::Join('|', 'p', 'q', 'r', 's', 't')
```

Result:

```
a,b,c,d,e
f-g-h-i-j
klmno
p|q|r|s|t
```

# Splitting Strings

Instead of joining strings, we can use the `-split` operator to split a string into an array.

```powershell
$string1 = "a,b,c,d,e"
$string2 = "f-g-h-i-j"
$string3 = 'monday,tuesday,wednesday,thursday,friday,saturday,sunday'
$string4 = 'one1two2three3four4five5six6seven7'

# Splitting based on comma with .Split()
$result1 = $string1.split(",")
Write-Host "Count of items split: " $result1.Count
Write-Host $result1
""

# Splitting based on hyphen with -split()
$result2 = $string2 -split "-"
Write-Host "Count of items split: " $result2.Count
""

# Splitting based on a part of the string with -split
$string3 -split "day"

# Split the string based on a try/catch block
# and limit the amount of items returned (3)
$string4 -split {
    try {
        [int]$_ -gt 1
    }
    catch {
        # Just ignore it
    }
}, 3
```

Result:

```
Count of items split:  5
a b c d e

Count of items split:  5

mon
,tues
,wednes
,thurs
,fri
,satur
,sun

one1two
three
four4five5six6seven7
```

In the first part the string is split based on the comma. The result is an array with 5 items.  
In the second part the string is split based on the hyphen with a different method.  
In the third part the string is split based on a part of a word.  
In the fourth part the string is split based on a try/catch block, but the amount of items returned is limited to 3.

# Formatting Strings

Formatting strings is something that can really make your code better readable.

```PowerShell
$firstName = "John"
$middleName = "Hubert"
$lastName = "Doe"

# Format the names
"{0} {1} {2}" -f $firstName, $middleName, $lastName

# Format the names but also do something else
"{0}.{1}@awesomecorp.com" -f $firstName.Substring(0, 1), $lastName
```

Result:

```
John Hubert Doe
J.Doe@awesomecorp.com
```

# Strings in Objects

With PowerShell we can do a lot of things with custom objects and we may want to use the values within.  
Using these values is something that in some cases need to be done in a certain way.

```powershell
# Create a simple object
$person = @{
    FirstName = 'John'
    LastName  = 'Doe'
}
# Let's see what we have
$person

# Use the object by referencing a property
Write-Host "`nHello, " $person.FirstName

# Setup the full name of the person using the properties
$fullName = $person.FirstName + ' ' + $person.LastName
Write-Host "`nHello, " $fullName

# Try to add the properties to a string
$fullName = "$person.FirstName $person.LastName"
Write-Host "`nHello, " $fullName # This will fail
# Let's add them the proper way
$fullName = "$($person.FirstName) $($person.LastName)"
Write-Host "Hello, " $fullName # This will work
```

Result:

```
Name                           Value
----                           -----
LastName                       Doe
FirstName                      John

Hello,  John

Hello,  John Doe

Hello,  System.Collections.Hashtable.FirstName System.Collections.Hashtable.LastName
Hello,  John Doe
```

As you can see the first two examples work pretty well. We can retrieve the values using the properties of the object.

In the third example we see we can't just add the properties to a string. We need to use the PowerShell notation to do this. 

In the fourth example we use the PowerShell notation to add the properties to a string.

# Replacing Strings

Replacing values in strings is something that is very handy in situation where you may use templates and want to replace certain values.
Or maybe you have a CSV file and want to replace the delimiter from a comma to a semi-colon.

You can either use the `.Replace()` method or the `-replace` operator.

```powershell
# Setup the query
$query = "SELECT * FROM [_SCHEMANAME_].[_TABLENAME_] WHERE `id` = _ID_";

# Replace the templated values using the .Replace() method
$query = $query.Replace("_SCHEMANAME_", "dbo");
$query = $query.Replace("_TABLENAME_", "tbl_test");
$query = $query.Replace("_ID_", "1");
$query

# Replace the delimiter for the CSV using the -replace operator
$csv = "a,b,c,d,e,f,g,h,i"
$csv -replace ",", ";"
```

Result:

```
SELECT * FROM [dbo].[tbl_test] WHERE id = 1
a;b;c;d;e;f;g;h;i
```



