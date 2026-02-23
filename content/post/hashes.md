---
title: Cryptographic Hashing
author: Sander Stad
date: '2026-02-23'
categories:
  - Example
weight: 175
---

Hashing is the process of converting data into a fixed-length string using a mathematical algorithm. The same input always produces the same output, but the output cannot be reversed to recover the original input. Hashes are commonly used to verify file integrity, store passwords securely, and detect data changes.

PowerShell provides built-in support for hashing through `Get-FileHash` and the .NET `[System.Security.Cryptography]` namespace.

---

# Get-FileHash

`Get-FileHash` computes the hash of a file. By default it uses SHA256.

```powershell
Get-FileHash -Path "C:\Temp\example.txt"
```

Result:

```
Algorithm       Hash                                                             Path
---------       ----                                                             ----
SHA256          3B4C5D6E7F8A9B0C1D2E3F4A5B6C7D8E9F0A1B2C3D4E5F6A7B8C9D0E1F2A3B4  C:\Temp\example.txt
```

## Choosing an Algorithm

The `-Algorithm` parameter lets you select from: `MD5`, `SHA1`, `SHA256`, `SHA384`, or `SHA512`.

```powershell
Get-FileHash -Path "C:\Temp\example.txt" -Algorithm MD5
Get-FileHash -Path "C:\Temp\example.txt" -Algorithm SHA1
Get-FileHash -Path "C:\Temp\example.txt" -Algorithm SHA512
```

Result:

```
Algorithm       Hash                                 Path
---------       ----                                 ----
MD5             D41D8CD98F00B204E9800998ECF8427E     C:\Temp\example.txt
SHA1            DA39A3EE5E6B4B0D3255BFEF95601890AFD  C:\Temp\example.txt
SHA512          CF83E1357EEFB8BDF1542850D66D8007D6...  C:\Temp\example.txt
```

> Note: MD5 and SHA1 are considered cryptographically weak and should not be used for security purposes. Prefer SHA256 or higher.

---

# Hashing a String

`Get-FileHash` works on files, but you can hash a string by converting it to a stream first.

```powershell
function Get-StringHash {
    param(
        [string]$Value,
        [string]$Algorithm = "SHA256"
    )

    $stream = [System.IO.MemoryStream]::new(
        [System.Text.Encoding]::UTF8.GetBytes($Value)
    )
    (Get-FileHash -InputStream $stream -Algorithm $Algorithm).Hash
}

Get-StringHash -Value "Hello, World!"
Get-StringHash -Value "Hello, World!" -Algorithm MD5
```

Result:

```
DFFD6021BB2BD5B0AF676290809EC3A53191DD81C7F70A4B28688A362182986F
65A8E27D8879283831B664BD8B7F0AD4
```

---

# Verifying File Integrity

A common use case is verifying a downloaded file matches its published checksum.

```powershell
$expectedHash = "3B4C5D6E7F8A9B0C1D2E3F4A5B6C7D8E9F0A1B2C3D4E5F6A7B8C9D0E1F2A3B4"
$actualHash   = (Get-FileHash -Path "C:\Temp\example.txt").Hash

if ($actualHash -eq $expectedHash) {
    Write-Host "File is valid. Hash matches."
} else {
    Write-Host "WARNING: Hash mismatch! File may be corrupted or tampered with."
    Write-Host "Expected: $expectedHash"
    Write-Host "Actual:   $actualHash"
}
```

Result (on a matching file):

```
File is valid. Hash matches.
```

Note that the `-eq` comparison is case-insensitive, so you don't need to normalise the case of the hash string. See [Case Sensitivity](/post/case-sensitivity) for more on this.

---

# Hashing Multiple Files

You can pipe a list of files to `Get-FileHash` to hash them all at once.

```powershell
Get-ChildItem -Path "C:\Temp" -File | Get-FileHash -Algorithm SHA256
```

Result:

```
Algorithm       Hash                                                             Path
---------       ----                                                             ----
SHA256          A1B2C3D4E5F6A7B8C9D0E1F2A3B4C5D6E7F8A9B0C1D2E3F4A5B6C7D8E9F0  C:\Temp\file1.txt
SHA256          B2C3D4E5F6A7B8C9D0E1F2A3B4C5D6E7F8A9B0C1D2E3F4A5B6C7D8E9F0A1  C:\Temp\file2.txt
SHA256          C3D4E5F6A7B8C9D0E1F2A3B4C5D6E7F8A9B0C1D2E3F4A5B6C7D8E9F0A1B2  C:\Temp\file3.txt
```

---

# Comparing Two Files

You can use hashes to quickly check whether two files are identical without comparing them byte by byte.

```powershell
$hash1 = (Get-FileHash -Path "C:\Temp\original.txt").Hash
$hash2 = (Get-FileHash -Path "C:\Temp\copy.txt").Hash

if ($hash1 -eq $hash2) {
    Write-Host "Files are identical."
} else {
    Write-Host "Files are different."
}
```

Result:

```
Files are identical.
```

---

# Using .NET Directly

For more control, or for hashing data that is not a file, you can use .NET's `System.Security.Cryptography` namespace.

```powershell
$data      = [System.Text.Encoding]::UTF8.GetBytes("Hello, World!")
$sha256    = [System.Security.Cryptography.SHA256]::Create()
$hashBytes = $sha256.ComputeHash($data)

# Convert byte array to hex string
$hashHex = [System.BitConverter]::ToString($hashBytes) -replace "-", ""
$hashHex
```

Result:

```
DFFD6021BB2BD5B0AF676290809EC3A53191DD81C7F70A4B28688A362182986F
```

---

# Summary

| Method | Use case |
|---|---|
| `Get-FileHash` | Hash files on disk, simple and built-in |
| `Get-FileHash -InputStream` | Hash strings or in-memory data |
| `[System.Security.Cryptography]` | Fine-grained control, any data type |

Prefer **SHA256** or higher for any security-sensitive purpose. Use MD5 only when interoperability with legacy systems requires it.
