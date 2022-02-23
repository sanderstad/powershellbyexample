---
title: Switch
author: Sander Stad
date: '2022-02-23'
categories:
  - Example
weight: 80
---

Switch statements are a way to execute different code based on different conditions.
This approach can be more efficient than using multiple if/elseif statements

```powershell
$month = 3

if ($month -eq 1) { Write-Host "January" }
elseif ($month -eq 2) { Write-Host "February" }
elseif ($month -eq 3) { Write-Host "March" }
elseif ($month -eq 4) { Write-Host "April" }
elseif ($month -eq 5) { Write-Host "May" }
elseif ($month -eq 6) { Write-Host "June" }
elseif ($month -eq 7) { Write-Host "July" }
elseif ($month -eq 8) { Write-Host "August" }
elseif ($month -eq 9) { Write-Host "September" }
elseif ($month -eq 10) { Write-Host "October" }
elseif ($month -eq 11) { Write-Host "November" }
elseif ($month -eq 12) { Write-Host "December" }
else { Write-Host "Invalid month" }

# Instead we can write the above as
switch ($month) {
    1 { Write-Host "January" }
    2 { Write-Host "February" }
    3 { Write-Host "March" }
    4 { Write-Host "April" }
    5 { Write-Host "May" }
    6 { Write-Host "June" }
    7 { Write-Host "July" }
    8 { Write-Host "August" }
    9 { Write-Host "September" }
    10 { Write-Host "October" }
    11 { Write-Host "November" }
    12 { Write-Host "December" }
    default { Write-Host "Invalid month" }
}
```

Result:

```
March
March
```

The result is the same but the code is much more readable.

## Wildcards

You can use wildcards in the condition of the case:

```powershell
# Using the -Wildcard parameter
$msg = "Error, the action failed"
switch -Wildcard ($msg) {
    "Error*" { "Action error" }
    "Warning*" { "Action warning" }
    "Successful*" { "Action succesfull" }
}

## Or use it in the conditions
$msg = "Error, the action failed"
switch ($msg) {
    { $_ -like "Error*" } { "Action error" }
    { $_ -like "Warning*" } { "Action warning" }
    { $_ -like "Successful*" } { "Action succesfull" }
}
```

Result:

```
Action error
Action error
```

Both have the same result, but with the last example you are more flexible to the operators you want to use.

## Multiple conditions

To use multiple expressions in a switch statement, you can use the `-and` and `-or` operators.

```powershell
switch ((Get-Date).Day) {
    { $_ -le 10 } { "Day of the month is lower than 10" }
    { $_ -gt 10 -and $_ -le 25 } { "Day of the month is between 10 and 25" }
    { $_ -gt 25 } { "Day of the month is greater than 25" }
}
```

Result:

```
Day of the month is between 10 and 25
```

