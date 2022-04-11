---
title: JSON
author: Sander Stad
date: '2022-04-11'
categories:
  - Example
weight: 230
---

If there is one thing that you want to do with PowerShell is handling JSON data.  
Because PowerShell is so flexible with custom objects, reading and writing JSON data becomes very easy and powerful.

# Reading JSON data

The parsing of JSON data is done using the `ConvertFrom-Json` command.

```powershell
$url = "https://gist.githubusercontent.com/sanderstad/1c47c1add7476945857bff4d8dc2be59/raw/d12f30e4aaf9d2ee18e4539b394a12e63dea0c9c/SampleJSON1.json"
$json = (New-Object System.Net.WebClient).DownloadString($url)

$data = $json | ConvertFrom-Json

$data
```

The result should look like this:

```
color  category type      code
-----  -------- ----      ----
black  hue      primary   @{rgba=System.Object[]; hex=#000}
white  value              @{rgba=System.Object[]; hex=#FFF}
red    hue      primary   @{rgba=System.Object[]; hex=#FF0}
blue   hue      primary   @{rgba=System.Object[]; hex=#00F}
yellow hue      primary   @{rgba=System.Object[]; hex=#FF0}
green  hue      secondary @{rgba=System.Object[]; hex=#0F0}
```

As you can see the data is transformed into a custom object. Any nested data deeper than one level is also transformed into a custom object and saved in the parent property. For example the `code` property is transformed into a custom object inside the parent custom object.

# Writing JSON data

Using the data as in the previous example, we can also convert it back to JSON and write the data to a file.  
To convert the custom object back to JSON, we use the `ConvertTo-Json` command.

```powershell
# Make sure you run the previous example first before running this one

$data | ConvertTo-Json | Out-File $env:temp\json.txt -Force
Get-Content $env:temp\json.txt
```

The result should look like this:

```
{
    "colors":  [
                   {
                       "color":  "black",
                       "category":  "hue",
                       "type":  "primary",
                       "code":  "@{rgba=System.Object[]; hex=#000}"
                   },
                   {
                       "color":  "white",
                       "category":  "value",
                       "code":  "@{rgba=System.Object[]; hex=#FFF}"
                   },
....
....
....
}
```

The data is converted to JSON and written to a file, but we have a problem.  
The color codes are not saved as strings but instead as the type it was imported as.

## Convert to a specific depth

Whenever you want to convert the data to JSON, you can specify the depth of the data.  
By default the depth is set to 2 which may not be obvious in PowerShell 5 because no warning will be given that the data will be truncated.  
In PowerShell 7 you will get a warning that the data will be truncated.  

To convert the data to JSON with a specific depth, you can use the `-Depth` parameter. Using the same data we had before, we can convert it to JSON with the correct depth.  
The correct depth for this JSON data is 3.

```powershell
$data | ConvertTo-Json -Depth 3 | Out-File $env:temp\json.txt -Force

```

The result should now look like this:

```
{
    "colors":  [
                   {
                       "color":  "black",
                       "category":  "hue",
                       "type":  "primary",
                       "code":  {
                                    "rgba":  "255 255 255 1",
                                    "hex":  "#000"
                                }
                   },
                   {
                       "color":  "white",
                       "category":  "value",
                       "code":  {
                                    "rgba":  "0 0 0 1",
                                    "hex":  "#FFF"
                                }
                   },
....
....
....
}                   
```

As you can see the data on the deeper levels is now written in the correct way.

