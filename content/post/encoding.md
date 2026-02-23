---
title: Encoding and Decoding
author: Sander Stad
date: '2026-02-23'
categories:
  - Example
weight: 185
---

Encoding converts data from one representation to another so it can be safely stored, transmitted, or processed. Decoding reverses that process. PowerShell has built-in support for several common encoding schemes.

---

# Base64

Base64 encodes binary data as printable ASCII text. It is widely used for embedding files in text formats, sending data in HTTP headers, and PowerShell's own `-EncodedCommand` parameter.

## Encoding a String

```powershell
$text = "Hello, World!"
$bytes = [System.Text.Encoding]::UTF8.GetBytes($text)
$encoded = [System.Convert]::ToBase64String($bytes)
$encoded
```

Result:

```
SGVsbG8sIFdvcmxkIQ==
```

## Decoding a String

```powershell
$encoded = "SGVsbG8sIFdvcmxkIQ=="
$bytes = [System.Convert]::FromBase64String($encoded)
$decoded = [System.Text.Encoding]::UTF8.GetString($bytes)
$decoded
```

Result:

```
Hello, World!
```

## Encoding a File

```powershell
$bytes = [System.IO.File]::ReadAllBytes("C:\Temp\image.png")
$encoded = [System.Convert]::ToBase64String($bytes)
$encoded | Set-Content -Path "C:\Temp\image.b64"
```

## Decoding a File

```powershell
$encoded = Get-Content -Path "C:\Temp\image.b64" -Raw
$bytes = [System.Convert]::FromBase64String($encoded)
[System.IO.File]::WriteAllBytes("C:\Temp\image-restored.png", $bytes)
```

## EncodedCommand

PowerShell's `-EncodedCommand` parameter accepts a Base64-encoded command, which is useful for passing complex scripts as a single-line argument (e.g. from a scheduler or another process). Note that `-EncodedCommand` requires **UTF-16LE** encoding, not UTF-8.

```powershell
$command = 'Write-Host "Hello from encoded command"'
$bytes   = [System.Text.Encoding]::Unicode.GetBytes($command)
$encoded = [System.Convert]::ToBase64String($bytes)

# Run it
powershell.exe -EncodedCommand $encoded
```

Result:

```
Hello from encoded command
```

---

# URL Encoding

URL encoding (percent-encoding) replaces characters that are not allowed in a URL with a `%` followed by their hex value. PowerShell can do this via the `System.Net.WebUtility` class or `System.Uri`.

## Encoding a URL String

```powershell
$text    = "hello world & more=stuff"
$encoded = [System.Net.WebUtility]::UrlEncode($text)
$encoded
```

Result:

```
hello+world+%26+more%3Dstuff
```

## Decoding a URL String

```powershell
$encoded = "hello+world+%26+more%3Dstuff"
$decoded = [System.Net.WebUtility]::UrlDecode($encoded)
$decoded
```

Result:

```
hello world & more=stuff
```

## URI Encoding (RFC 3986)

`System.Uri::EscapeDataString` follows RFC 3986 and is preferred when building query strings for REST APIs, as it encodes spaces as `%20` rather than `+`.

```powershell
$text    = "hello world & more=stuff"
$encoded = [System.Uri]::EscapeDataString($text)
$encoded

$decoded = [System.Uri]::UnescapeDataString($encoded)
$decoded
```

Result:

```
hello%20world%20%26%20more%3Dstuff
hello world & more=stuff
```

---

# HTML Encoding

HTML encoding replaces characters such as `<`, `>`, and `&` with their HTML entity equivalents, preventing them from being interpreted as markup.

## Encoding HTML

```powershell
$text    = '<script>alert("XSS")</script> & other <stuff>'
$encoded = [System.Net.WebUtility]::HtmlEncode($text)
$encoded
```

Result:

```
&lt;script&gt;alert(&quot;XSS&quot;)&lt;/script&gt; &amp; other &lt;stuff&gt;
```

## Decoding HTML

```powershell
$encoded = "&lt;b&gt;Hello&lt;/b&gt; &amp; welcome"
$decoded = [System.Net.WebUtility]::HtmlDecode($encoded)
$decoded
```

Result:

```
<b>Hello</b> & welcome
```

---

# Character Encoding

Character encoding determines how text is represented as bytes. This is especially important when reading and writing files. See the [Reading Files](/post/reading-files) and [Writing Files](/post/writing-files) pages for file I/O examples.

## Common Encodings in PowerShell

```powershell
# List available encoding names
[System.Text.Encoding]::GetEncodings() | Select-Object -Property Name, DisplayName
```

The most common encodings you will encounter:

| Name | Description |
|---|---|
| `UTF8` | Default in PowerShell 7, supports all Unicode characters |
| `Unicode` | UTF-16LE, default in Windows PowerShell 5.1 |
| `ASCII` | 7-bit, English characters only |
| `UTF32` | 4 bytes per character |

## Writing a File with a Specific Encoding

```powershell
# UTF-8 without BOM (common for cross-platform files)
$content = "Héllo Wörld"
[System.IO.File]::WriteAllText("C:\Temp\output.txt", $content, [System.Text.Encoding]::UTF8)

# UTF-8 with BOM (Windows default)
$content | Out-File -FilePath "C:\Temp\output-bom.txt" -Encoding utf8BOM
```

## Reading a File with a Specific Encoding

```powershell
# Explicitly specify encoding when reading
$content = Get-Content -Path "C:\Temp\output.txt" -Encoding UTF8
$content
```

---

# Summary

| Type | Encode | Decode |
|---|---|---|
| Base64 | `[Convert]::ToBase64String()` | `[Convert]::FromBase64String()` |
| URL (`+` for spaces) | `[WebUtility]::UrlEncode()` | `[WebUtility]::UrlDecode()` |
| URL (RFC 3986) | `[Uri]::EscapeDataString()` | `[Uri]::UnescapeDataString()` |
| HTML | `[WebUtility]::HtmlEncode()` | `[WebUtility]::HtmlDecode()` |
| Character encoding | `[Encoding]::UTF8.GetBytes()` | `[Encoding]::UTF8.GetString()` |
