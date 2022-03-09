---
title: Error Handling
author: Sander Stad
date: '2022-03-09'
categories:
  - Example
weight: 160
---

The try/catch blocks in PowerShell are used to handle terminating error. With the try/catch block you also have a finally keyword that will be executed even when an error is found.

## General

The example below is a standard way to do a try/catch block

```powershell
try {
    # This will generate an error
    1/0
    Write-Host "This is executed after the error"
} catch {
    # Catch all errors
    Write-Host "Oh oh! Error occurred.`n$_"
}
```

Result:

```
Oh oh! Error occurred.
Attempted to divide by zero.
```

You can see that an error was caught and that the error was returned as well.

## Specific error catches

```powershell
try {
    # This will generate an error
    1/0
    Write-Host "This is executed after the error"
} catch [System.DivideByZeroException] {
    # Catch all errors
    Write-Host "Divide by zero error occurred.`n$_"
} catch {
    # Catch all errors
    Write-Host "Oh oh! Another error occurred.`n$_"
}
```

Result:

```
Divide by zero error occurred.
Attempted to divide by zero.
```

As you can see the specific error is returned instead of the general catch.

## Using finally

```powershell
try {
    # This will generate an error
    1/1
    Write-Host "This is executed after the error"
} catch [System.DivideByZeroException] {
    # Catch all errors
    Write-Host "Divide by zero error occurred.`n$_"
} catch {
    # Catch all errors
    Write-Host "Oh oh! Another error occurred.`n$_"
} finally {
    Write-Host "Finally!"
}
```

Result:

```
Divide by zero error occurred.
Attempted to divide by zero.
Finally!
```

As you can see the finally is always executed whether or not we have an error.
