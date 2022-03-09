---
title: Reading Files
author: Sander Stad
date: '2022-03-09'
categories:
  - Example
weight: 170
---

To demonstrate reading files we need sample files first.

```powershell
# Let's download a sample file just for the example
$url = "https://gist.githubusercontent.com/sanderstad/7b9593f7f30abb9f17f9026c74ed9c68/raw/d4406c4cbbc427e15fc9d6d92f8bcf3c72a1e70a/samplefile1.txt"
$filePath = Join-Path -Path $env:temp -ChildPath "samplefile1.txt"

Invoke-WebRequest -Uri $url -OutFile $filePath

$url = "https://gist.githubusercontent.com/sanderstad/f59996889fc3ec794d325ad2162648f8/raw/5353480009bd714f9764a093b52f0fabff1078fd/samplefile2.csv"
$filePath = Join-Path -Path $env:temp -ChildPath "samplefile2.txt"

Invoke-WebRequest -Uri $url -OutFile $filePath
```

## Get-Content

The easiest way to read files in PowerShell is by using the the `Get-Content` command.

```powershell
# Get the content
Get-Content -Path (Join-Path -Path $env:temp -ChildPath "samplefile1.txt")
```

Result:

```
Utilitatis causa amicitia est quaesita.
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Collatio igitur ista te nihil iuvat. Honesta oratio, Socratica, Platonis etiam. Primum in nostrane potestate est, quid meminerimus? Duo Reges: constructio interrete. Quid, si etiam iucunda memoria est praeteritorum malorum? Si quidem, inquit, tollerem, sed relinquo. An nisi populari fama?

Quamquam id quidem licebit iis existimare, qui legerint. Summum a vobis bonum voluptas dicitur. At hoc in eo M. Refert tamen, quo modo. Quid sequatur, quid repugnet, vident. Iam id ipsum absurdum, maximum malum neglegi.
```

```powershell
$content = Get-Content -Path (Join-Path -Path $env:temp -ChildPath "samplefile1.txt")

$content.GetType()
$content.Count
```

Result:

```
IsPublic IsSerial Name                                     BaseType
-------- -------- ----                                     --------
True     True     Object[]                                 System.Array

4
```

What PowerShell has done is get the content as an array and each row that end with a newline character will be a new entry.

To get the first few rows we can do the following

```powershell
$content = Get-Content -Path (Join-Path -Path $env:temp -ChildPath "samplefile1.txt")

$content | Select-Object -First 2
```

Result:

```
Utilitatis causa amicitia est quaesita.
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Collatio igitur ista te nihil iuvat. Honesta oratio, Socratica, Platonis etiam. Primum in nostrane potestate est, quid meminerimus? Duo Reges: constructio interrete. Quid, si etiam iucunda memoria est praeteritorum malorum? Si quidem, inquit, tollerem, sed relinquo. An nisi populari fama?
```

## Import-CSV

When you have a specific CSV file, you can use the `Import-Csv` command to interpret the data as CSV.

```powershell
Import-Csv -Path (Join-Path -Path $env:temp -ChildPath "samplefile2.txt")
```

Result:

```
Month   : May
Average : 0.1
2005    : 0
2006    : 0
2007    : 1
2008    : 1
2009    : 0
2010    : 0
2011    : 0
2012    : 2
2013    : 0
2014    : 0
2015    : 0

Month   : Jun
Average : 0.5
2005    : 2
2006    : 1
2007    : 1
2008    : 0
2009    : 0
.........
```

As you can see it returns each and every row as a single object with the data.
