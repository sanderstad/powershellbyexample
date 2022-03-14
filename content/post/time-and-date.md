---
title: Time and Date
author: Sander Stad
date: '2022-03-14'
categories:
  - Example
weight: 190
---


In many operations within PowerShell we need time and date information.
When date and time values are being returned that is done with the `[datetime]` data type.
The format that's being returned depends on the cultural settings of the computer.

## General

To get the current date and time, use the ` Get-Date` command.

```powershell
Get-Date
```

Result:

```
Monday, March 14, 2022 4:40:48 PM
```

## Date parts

To get specific values from the command we can use the properties in the `[datetime]` type.

```powershell
$today = Get-Date
"Full Date: `t$today"
"Date: `t`t$($today.Date)"
"Year: `t`t$($today.Year)"
"Month: `t`t$($today.Month)"
"Day: `t`t$($today.Day)"
"Day of the Week: $($today.DayOfWeek)"
"Day of the Year: $($today.DayOfYear)"
"Hour: `t`t$($today.Hour)"
"Minute: `t$($today.Minute)"
"Second: `t$($today.Second)"
"Millisecond: $($today.Millisecond)"
```

Result:
```
Full Date:  03/14/2022 20:25:15
Date:       03/14/2022 00:00:00
Year:       2022
Month:      3
Day:        14
Day of the Week: Monday
Day of the Year: 73
Hour:       20
Minute:     25
Second:     15
Millisecond: 322
```

## Formatting dates

Sometimes we need to format the date. This can be done with the `-f` parameter.
This parameter takes in a string and replaces the placeholders with the values from the `[datetime]` type.

```powershell
$date = Get-Date
$date
$date -f "yyyyMMdd"
```

Result:

```
Monday, March 14, 2022 4:47:03 PM
03/14/2022 16:47:03
```

## Adding and subtracting 

It is possible to add and subtract dates in powershell. The functions may seem confusing but you can add using positive numbers and subtract using negative numbers.

```powershell
$today = Get-Date
$yesterday = $today.AddDays(-1)
$tomorrow = $today.AddDays(1)
"Today: `t`t$today"
"Yesterday: `t$yesterday"
"Tomorrow: `t$tomorrow"
```

Result:

```
Today:          03/14/2022 20:33:43
Yesterday:      03/13/2022 20:33:43
Tomorrow:       03/15/2022 20:33:43
```

The same actions can be done with the other add functions like:

| Functions       | Description | Type                                   |
| --------------- | ----------- | -------------------------------------- |
| AddDays         | Method      | datetime AddDays(double value)         |
| AddHours        | Method      | datetime AddHours(double value)        |
| AddMilliseconds | Method      | datetime AddMilliseconds(double value) |
| AddMinutes      | Method      | datetime AddMinutes(double value)      |
| AddMonths       | Method      | datetime AddMonths(int months)         |
| AddSeconds      | Method      | datetime AddSeconds(double value)      |
| AddTicks        | Method      | datetime AddTicks(long value)          |
| AddYears        | Method      | datetime AddYears(int value)           |
