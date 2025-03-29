# **YAML**  

## **What is YAML?**  
YAML, originally an acronym for "Yet Another Markup Language," now stands for "YAML Ain’t Markup Language." It is a **human-readable data serialization format** commonly used for **configuration files**, **data exchange**, and **defining infrastructure-as-code**. YAML emphasizes readability and simplicity, making it ideal for DevOps, Kubernetes, CI/CD pipelines, and cloud configurations.

### **Why Use YAML?**  
✅ **Human-readable** – Easy to write and understand  
✅ **Minimalistic** – Uses indentation instead of complex tags  
✅ **Supports complex data structures** – Handles lists, dictionaries, and nested objects  
✅ **Widely adopted** – Used in Docker Compose, Kubernetes, Ansible, GitHub Actions, etc.

---

## **1. YAML Syntax Basics**  

### **1.1 Rules**  
- **Key-value pairs (`key: value`)** define mappings.  
- **Indentation (spaces, NOT tabs)** represents hierarchy.  
- **Lists use `-` (hyphen) as a prefix.**  
- **Strings do not require quotes unless needed.**  
- **Comments use `#`** to add explanatory notes.  

### **1.2 Example YAML File**  
```yaml
# User Profile Example
name: John Doe
age: 30
is_active: true
skills:
  - Python
  - JavaScript
  - Go
contact:
  email: john@example.com
  phone: 123-456-7890
```

---

## **2. YAML Data Structures**  

### **2.1 Scalars (Basic Data Types)**  
YAML supports simple data types like:  
- **Strings**: `"Hello World"`, `'YAML'`
- **Numbers**: `42`, `3.14`, `0xFF`
- **Boolean**: `true`, `false`
- **Null**: `null`, `~`

```yaml
string_example: "Hello, YAML!"
integer_example: 100
float_example: 3.14
boolean_example: true
null_example: null
```

### **2.2 Lists (Sequences)**  
A YAML list is created using `-` (hyphen).  

#### **Example: List of Values**  
```yaml
fruits:
  - Apple
  - Banana
  - Orange
```

#### **Inline List Format**  
```yaml
fruits: [Apple, Banana, Orange]
```

### **2.3 Dictionaries (Mappings)**  
Mappings (key-value pairs) allow nesting.  

```yaml
person:
  name: Alice
  age: 25
  address:
    city: New York
    country: USA
```

#### **Inline Dictionary Format**  
```yaml
person: { name: Alice, age: 25, address: { city: New York, country: USA } }
```

---

## **3. Advanced YAML Features**  

### **3.1 Multiline Strings**
YAML supports two ways to handle multiline text:  
1. **Block Scalars (`|`)** → Preserves newlines  
2. **Folded Scalars (`>`)** → Folds into a single line

```yaml
literal_block: |
  This is a multiline
  string with new lines
  preserved.

folded_block: >
  This is a folded
  string that will be
  converted into a single line.
```

**Output:**  
```yaml
literal_block: "This is a multiline\nstring with new lines\npreserved."
folded_block: "This is a folded string that will be converted into a single line."
```

---

### **3.2 Anchors and Aliases (Reusability)**  
- **`&`** creates an anchor.  
- **`*`** references (alias) an anchor.  

```yaml
default_config: &default
  retry: 3
  timeout: 30
  debug: false

service1:
  <<: *default
  url: "https://api.service1.com"

service2:
  <<: *default
  url: "https://api.service2.com"
  debug: true
```

**Result:**  
`service1` and `service2` inherit from `default_config`, but `service2` overrides `debug`.

---

### **3.3 Environment Variables & Dynamic Values**  
You can reference environment variables dynamically:  

```yaml
database:
  user: ${DB_USER}
  password: ${DB_PASSWORD}
  host: localhost
  port: 5432
```

If used in Docker Compose or Kubernetes, these values are substituted at runtime.

---

## **4. YAML in DevOps & Cloud Configurations**  

### **4.1 Docker Compose Example**  
```yaml
version: '3.8'
services:
  web:
    image: nginx
    ports:
      - "80:80"
  database:
    image: postgres
    environment:
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: secret
```

---

### **4.2 Kubernetes Configuration Example**  
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-app
  labels:
    app: web
spec:
  containers:
    - name: web
      image: nginx
      ports:
        - containerPort: 80
```

---

### **4.3 GitHub Actions CI/CD Pipeline Example**  
```yaml
name: Build and Deploy

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Install Dependencies
        run: npm install

      - name: Build Application
        run: npm run build
```

---

## **5. YAML vs Other Formats**  

| Feature      | YAML | JSON | XML |
|-------------|------|------|-----|
| Readability | ✅ Easy to read | ❌ Harder | ❌ Harder |
| Verbosity   | ✅ Minimal | ✅ Medium | ❌ High |
| Comments    | ✅ Yes (`#`) | ❌ No | ✅ Yes (`<!-- -->`) |
| Data Types  | ✅ Rich | ✅ Rich | ✅ Rich |
| Parsing Complexity | ✅ Simple | ✅ Simple | ❌ Complex |

### **Example Comparison**  
#### **XML**
```xml
<Employees>
    <Employee>
        <name>John Doe</name>
        <department>Engineering</department>
        <country>USA</country>
    </Employee>
</Employees>
```

#### **JSON**
```json
{
    "Employees": [
        {
            "name": "John Doe",
            "department": "Engineering",
            "country": "USA"
        }
    ]
}
```

#### **YAML**
```yaml
Employees:
  - name: John Doe
    department: Engineering
    country: USA
```

---

## **6. YAML Parsing in .NET (C#)**  

Using **`YamlDotNet`**:  

```csharp
using System;
using System.IO;
using YamlDotNet.Serialization;
using YamlDotNet.Serialization.NamingConventions;

class Program
{
    static void Main()
    {
        var yaml = @"
person:
  name: Alice
  age: 25
  skills:
    - C#
    - .NET
    - Docker
";

        var deserializer = new DeserializerBuilder()
            .WithNamingConvention(CamelCaseNamingConvention.Instance)
            .Build();

        var obj = deserializer.Deserialize<dynamic>(yaml);
        Console.WriteLine(obj["person"]["name"]); // Alice
    }
}
```

---

## **7. Best Practices for YAML**  

✅ **Use Consistent Indentation** (2 or 4 spaces, no tabs).  
✅ **Keep Lines Short** (Avoid deeply nested structures).  
✅ **Use Comments** (`#`) to explain configurations.  
✅ **Prefer Anchors & Aliases** for reuse instead of duplicating content.  
✅ **Validate YAML** using `yamllint` or online YAML parsers.  

**Command to validate YAML:**  
```sh
yamllint config.yaml
```

---

## **Conclusion**  
YAML is a powerful yet simple configuration format widely used in DevOps, cloud deployments, and CI/CD pipelines. Its readability, flexibility, and hierarchical structure make it a great choice for managing structured data.