---
title: Handling XML
author: Sander Stad
date: '2023-12-12'
categories:
  - Example
weight: 200
---

XML, or eXtensible Markup Language, is a versatile data format widely used for storing and exchanging structured information. In the realm of PowerShell, XML becomes a potent ally, offering a robust mechanism for handling and manipulating data in a hierarchical and human-readable way. In this article, we'll explore the integration of XML with PowerShell, unraveling the simplicity and efficacy it brings to scriptwriters.

## Creating XML with PowerShell

PowerShell provides seamless ways to generate XML content, both manually and programmatically. 

Let's embark on a journey by creating a basic XML structure using PowerShell's intrinsic capabilities:

```powershell
# Creating a simple XML document
$xmlDocument = New-Object System.Xml.XmlDocument

# Adding a root element
$rootElement = $xmlDocument.CreateElement("Root")
$xmlDocument.AppendChild($rootElement)

# Adding child elements
$childElement1 = $xmlDocument.CreateElement("Child1")
$childElement1.InnerText = "Value1"
$rootElement.AppendChild($childElement1)

$childElement2 = $xmlDocument.CreateElement("Child2")
$childElement2.InnerText = "Value2"
$rootElement.AppendChild($childElement2)

# Save the XML to a file
$xmlDocument.Save("C:\Path\To\Your\File.xml")
```

Result:

```xml
<Root>
  <Child1>Value1</Child1>
  <Child2>Value2</Child2>
</Root>
```

In this example, we create an XML document, add a root element named "Root," and populate it with two child elements, each containing a distinct value. The resulting XML is then saved to a specified file path.

## Reading XML with PowerShell

Reading XML in PowerShell is an intuitive process, allowing seamless navigation through the document's hierarchical structure. 

Let's explore how to extract information from an XML file:

```powershell
# Load XML from a file
$xmlFilePath = "C:\Path\To\Your\File.xml"
$xmlContent = Get-Content -Path $xmlFilePath
$xmlDocument = [xml]$xmlContent

# Accessing elements
$rootValue = $xmlDocument.Root.InnerText
$child1Value = $xmlDocument.Root.Child1.InnerText
$child2Value = $xmlDocument.Root.Child2.InnerText

# Displaying values
Write-Host "Root Value: $rootValue"
Write-Host "Child1 Value: $child1Value"
Write-Host "Child2 Value: $child2Value"
```

Result:

```
C:\> Write-Host "Root Value: $rootValue"
Root Value: Value1Value2
C:\> Write-Host "Child1 Value: $child1Value"
Child1 Value:
C:\> Write-Host "Child2 Value: $child2Value"
Child2 Value:
```

In this snippet, we load an XML file, cast it to an XML type, and then access specific elements and their corresponding values. The result is a straightforward display of the extracted data.

## Modifying XML with PowerShell

PowerShell empowers users to dynamically modify XML content, offering unparalleled flexibility in handling changing data requirements. 

Let's delve into an example where we update the value of a child element:

```powershell
# Load XML from a file
$xmlFilePath = "C:\Path\To\Your\File.xml"
$xmlContent = Get-Content -Path $xmlFilePath
$xmlDocument = [xml]$xmlContent

# Modify a child element's value
$xmlDocument.Root.Child1 = "New Value"

# Save the modified XML
$xmlDocument.Save($xmlFilePath)
```

Result:

```xml
<Root>
  <Child1>New Value</Child1>
  <Child2>Value2</Child2>
</Root>
```

In this scenario, we load the XML file, update the value of a specific child element (Child1), and then save the modified XML content back to the file.