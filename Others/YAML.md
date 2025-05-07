## 🧾 What is YAML?

**YAML** (YAML Ain’t Markup Language) is a **data serialization language** that's:

* **Human-readable**
* Used for **configuration files**, data exchange, and automation tools
* Designed to be simple and expressive

---

## 🏗️ YAML Data Structures & Syntax

YAML supports **three core data types**:

1. **Scalars** (strings, numbers, booleans)
2. **Sequences** (arrays/lists)
3. **Mappings** (dictionaries/hashes)

---

### ✅ 1. **Scalars (Primitive values)**

```yaml
name: Alice
age: 30
active: true
height: 5.7
```

* Strings don’t need quotes unless they include special characters.
* Boolean values: `true`, `false`
* Null value: `null` or `~`

---

### ✅ 2. **Sequences (Arrays / Lists)**

```yaml
skills:
  - Python
  - .NET
  - AWS
```

You can also use **inline style**:

```yaml
skills: [Python, .NET, AWS]
```

---

### ✅ 3. **Mappings (Dictionaries / Key-Value Pairs)**

```yaml
address:
  street: 123 Main St
  city: New York
  zip: 10001
```

Or inline:

```yaml
address: { street: 123 Main St, city: New York, zip: 10001 }
```

---

### ✅ 4. **Nested Structures**

```yaml
employee:
  name: Alice
  age: 30
  skills:
    - Python
    - Java
  address:
    city: Boston
    zip: 02115
```

---

### ✅ 5. **Comments**

```yaml
# This is a comment
name: Bob  # Inline comment
```

---

### ✅ 6. **Multiline Strings**

```yaml
description: |
  This is a long description.
  It spans multiple lines.
  YAML keeps the formatting.

summary: >
  This is a folded string
  that joins lines with spaces.
```

* `|` → Preserves line breaks
* `>` → Converts newlines to spaces

---

## 🔧 Real-Life Example: GitHub Actions Workflow

```yaml
name: CI Pipeline

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Setup .NET
        uses: actions/setup-dotnet@v3
        with:
          dotnet-version: 8.0.x

      - name: Build
        run: dotnet build
```

---

## 📌 Summary

| Feature     | YAML                                            |
| ----------- | ----------------------------------------------- |
| Data Types  | Scalars, Lists, Dictionaries                    |
| Uses        | Config files, CI/CD, Kubernetes, Docker Compose |
| Syntax      | Indentation-based, no braces                    |
| File Ext    | `.yaml` or `.yml`                               |
| Readability | Very high (human-friendly)                      |

---

## 📄 YAML vs JSON vs XML – Visual Cheat Sheet

Let’s use an example representing a person with name, age, skills, and address:

### ✅ YAML (Human-friendly, clean syntax)

```yaml
name: Alice
age: 30
skills:
  - Python
  - AWS
  - .NET
address:
  city: Boston
  zip: 02115
```

---

### ✅ JSON (Strict, machine-friendly)

```json
{
  "name": "Alice",
  "age": 30,
  "skills": ["Python", "AWS", ".NET"],
  "address": {
    "city": "Boston",
    "zip": "02115"
  }
}
```

---

### ✅ XML (Verbose, markup language)

```xml
<person>
  <name>Alice</name>
  <age>30</age>
  <skills>
    <skill>Python</skill>
    <skill>AWS</skill>
    <skill>.NET</skill>
  </skills>
  <address>
    <city>Boston</city>
    <zip>02115</zip>
  </address>
</person>
```

---

## 🧠 Summary Table

| Feature            | **YAML**              | **JSON**                | **XML**                     |
| ------------------ | --------------------- | ----------------------- | --------------------------- |
| **Syntax Style**   | Indentation-based     | Braces and brackets     | Tags                        |
| **Readability**    | High (human-friendly) | Medium                  | Low (verbose)               |
| **File Size**      | Small                 | Medium                  | Large                       |
| **Comments**       | ✅ Supports `#`        | ❌ Not supported         | ✅ Supported with `<!-- -->` |
| **Data Types**     | Scalars, lists, maps  | Strings, numbers, lists | Text, attributes            |
| **Use Case**       | Configs, DevOps tools | APIs, Web data          | Legacy systems, SOAP        |
| **File Extension** | `.yml`, `.yaml`       | `.json`                 | `.xml`                      |
| **Schema Support** | Optional              | Optional                | Strong schema support       |
| **Parsing Speed**  | Slowest               | Fast                    | Slow                        |

---

### ✅ When to Use Each:

* **YAML**:
  Ideal for **configuration files** and DevOps tools like Kubernetes, Ansible, GitHub Actions, Docker Compose.

* **JSON**:
  Best for **APIs**, **JavaScript-based web apps**, and lightweight data exchange.

* **XML**:
  Used in **enterprise systems**, legacy integrations, and **SOAP-based web services**.

---
