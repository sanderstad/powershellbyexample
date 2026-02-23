---
title: Case Sensitivity
author: Sander Stad
date: '2026-02-23'
categories:
  - Example
weight: 165
---

PowerShell is **case-insensitive by default**. This means that commands, variable names, string comparisons, and operators all work regardless of upper or lower case. However, there are many situations where case sensitivity matters, and PowerShell provides explicit ways to control it.

---

# Comparison Operators

All of PowerShell's comparison operators come in three flavors:

| Variant | Prefix | Behavior |
|---|---|---|
| Default | none | Case-insensitive |
| Case-insensitive | `i` | Explicitly case-insensitive |
| Case-sensitive | `c` | Explicitly case-sensitive |

## Equality (`-eq`)

```powershell
# Default (case-insensitive)
"PowerShell" -eq "powershell"   # True
"PowerShell" -ieq "powershell"  # True (explicit)
"PowerShell" -ceq "powershell"  # False (case-sensitive)
```

Result:

```
True
True
False
```

## Inequality (`-ne`)

```powershell
"PowerShell" -ne "powershell"   # False
"PowerShell" -ine "powershell"  # False (explicit)
"PowerShell" -cne "powershell"  # True (case-sensitive)
```

Result:

```
False
False
True
```

---

# Wildcard Matching (`-like`)

The `-like` operator supports `*` and `?` wildcards and is also case-insensitive by default.

```powershell
"Hello World" -like "hello*"    # True
"Hello World" -ilike "hello*"   # True (explicit)
"Hello World" -clike "hello*"   # False (case-sensitive)
"Hello World" -clike "Hello*"   # True
```

Result:

```
True
True
False
True
```

Similarly, `-notlike`, `-inotlike`, and `-cnotlike` follow the same pattern.

---

# Regular Expression Matching (`-match`)

The `-match` operator tests a string against a regex pattern and is case-insensitive by default.

```powershell
"PowerShell" -match "powershell"    # True
"PowerShell" -imatch "powershell"   # True (explicit)
"PowerShell" -cmatch "powershell"   # False (case-sensitive)
"PowerShell" -cmatch "PowerShell"   # True
```

Result:

```
True
True
False
True
```

See the [Regular Expressions](/post/regex) page for more detail on regex patterns.

---

# Contains and In Operators

The `-contains` and `-in` operators check for membership in a collection and are case-insensitive by default.

```powershell
$fruits = @("Apple", "Banana", "Cherry")

$fruits -contains "apple"    # True
$fruits -icontains "apple"   # True (explicit)
$fruits -ccontains "apple"   # False (case-sensitive)
$fruits -ccontains "Apple"   # True

"banana" -in $fruits         # True
"banana" -cin $fruits        # False (case-sensitive)
```

Result:

```
True
True
False
True
True
False
```

---

# Switch Statement

The `switch` statement is case-insensitive by default. Use the `-CaseSensitive` parameter to enforce case sensitivity.

```powershell
$value = "Hello"

# Case-insensitive (default)
switch ($value) {
    "hello" { "Matched (case-insensitive)" }
    "HELLO" { "Also matched" }
}
```

Result:

```
Matched (case-insensitive)
Also matched
```

```powershell
# Case-sensitive
switch -CaseSensitive ($value) {
    "hello" { "Lowercase match" }
    "Hello" { "Exact match" }
    "HELLO" { "Uppercase match" }
}
```

Result:

```
Exact match
```

See the [Switch](/post/switch) page for more examples.

---

# Select-String

`Select-String` searches for text in strings or files. By default it is case-insensitive. Add `-CaseSensitive` to change this.

```powershell
$lines = @(
    "Error: disk full",
    "error: permission denied",
    "WARNING: low memory",
    "Info: service started"
)

# Case-insensitive (default)
$lines | Select-String "error"
```

Result:

```
Error: disk full
error: permission denied
```

```powershell
# Case-sensitive: only exact case matches
$lines | Select-String -CaseSensitive "error"
```

Result:

```
error: permission denied
```

---

# Sort-Object

`Sort-Object` sorts in case-insensitive order by default. Use `-CaseSensitive` to distinguish between uppercase and lowercase values.

```powershell
$names = @("Banana", "apple", "Cherry", "cherry", "Apple", "banana")

# Case-insensitive (default)
$names | Sort-Object
```

Result:

```
apple
Apple
Banana
banana
Cherry
cherry
```

```powershell
# Case-sensitive: uppercase letters sort before lowercase
$names | Sort-Object -CaseSensitive
```

Result:

```
apple
Apple
banana
Banana
cherry
Cherry
```

See the [Sorting](/post/sorting) page for more sorting examples.

---

# Hashtables

Hashtable keys in PowerShell are case-insensitive by default. This means `$ht["Key"]` and `$ht["key"]` refer to the same entry.

```powershell
$ht = @{ Name = "Alice"; Age = 30 }

$ht["name"]   # Alice
$ht["NAME"]   # Alice
$ht["Name"]   # Alice
```

Result:

```
Alice
Alice
Alice
```

To create a case-sensitive hashtable, use a `Dictionary` with `StringComparer.Ordinal`.

```powershell
$caseSensitive = [System.Collections.Generic.Dictionary[string,object]]::new(
    [System.StringComparer]::Ordinal
)
$caseSensitive["Key"] = "Uppercase K"
$caseSensitive["key"] = "Lowercase k"

$caseSensitive["Key"]   # Uppercase K
$caseSensitive["key"]   # Lowercase k
```

Result:

```
Uppercase K
Lowercase k
```

See the [Hashtables](/post/hashtables) page for more examples.

---

# Summary

| Feature | Default | Case-sensitive option |
|---|---|---|
| `-eq`, `-ne`, `-gt`, `-lt` | Case-insensitive | Add `c` prefix: `-ceq`, `-cne` |
| `-like`, `-notlike` | Case-insensitive | Add `c` prefix: `-clike`, `-cnotlike` |
| `-match`, `-notmatch` | Case-insensitive | Add `c` prefix: `-cmatch`, `-cnotmatch` |
| `-contains`, `-in` | Case-insensitive | Add `c` prefix: `-ccontains`, `-cin` |
| `switch` | Case-insensitive | `-CaseSensitive` parameter |
| `Select-String` | Case-insensitive | `-CaseSensitive` parameter |
| `Sort-Object` | Case-insensitive | `-CaseSensitive` parameter |
| Hashtable keys | Case-insensitive | Use `StringComparer.Ordinal` |