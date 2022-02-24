---
title: Functions
author: Sander Stad
date: '2022-02-22'
categories:
  - Example
weight: 120
---

Functions are not required in PowerShell, but when your code becomes repetitive you should consider using functions.  
Also when creating PowerShell modules you should really consider putting code into functions to make your code more readable.

To create a function we use the `function` keyword:

```powershell
function writeHelloWorld() {
    Write-Host "Hello World!"
}

writeHelloWorld
```

Result:

```
Hello World!
```

Obviously this function is not very useful, but it is a good example how easy it is to create a function.

## Parameters

Parameters is where the magic happens. When you create a function you can pass parameters to it.

```powershell
function writeMessage {
    param(
        [string]$Message
    )

    Write-Host "Message: $message"
}

writeMessage "Hello World!"
writeMessage -message "Who is there?"
```

Result:

```
Message: Hello World!
Message: Who is there?
```

Notice that we can pass parameters with and without specifying the parameter.

## Adding advanced functionality

To make a function more advanced we can add `[CmdletBinding()]` to the function.  
This enabled you to support for:

* Confirming for impact
* Help URI
* Support paging
* Should process
* Positional binding

```powershell
function writeMessage {
    param(
        [Parameter(Mandatory = $true, Position = 1, HelpMessage = "The message to write")]
        [string]$Message
    )

    process {
        Write-Host "Message: $message"
    }
}

writeMessage "Hello World!"
writeMessage
```

Result:

```
Message: Hello World!
cmdlet writeMessage at command pipeline position 1
Supply values for the following parameters:
Message:
writeMessage: C:\temp\temp.ps1:14:1
Line |
  14 |  writeMessage
     |  ~~~~~~~~~~~~
     | Cannot bind argument to parameter 'Message' because it is an empty string.
```

Notice that the function also includes the `process` keyword. 

## Begin, process and End

These blocks are not required but they are very useful when you want to execute code before or after the function.

*Begin* is used for one-time processing before the rest of the function is executed.
*Process* is used for processing the function.
*End* is used for one-time processing after the rest of the function is executed.

```powershell
function writeMessage {
    [CmdLetBinding()]
    param(
        [Parameter(Mandatory = $true, Position = 1, HelpMessage = "The message to write")]
        [string]$Message
    )

    begin {
        Write-Verbose "Beginning of script"
        if (($null -eq $Message) -or ($Message -eq "")) {
            throw "Message cannot be empty";
        }
    }

    process {
        Write-Host "Message: $message"
    }

    end {
        Write-Host "End of script"
    }
}

writeMessage "Hello World!" -Verbose
```

Result:

```
Beginning of script
Message: Hello World!
End of script
```

For more information about advanced function go to the Microsoft Docs website:

- [Functions](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_functions?view=powershell-7.2)
- [Advanced Functions](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_functions_advanced?view=powershell-7.2)
- [Advanced Parameters](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_functions_advanced_parameters?view=powershell-7.2)