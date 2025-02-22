### **Memory Allocation for Different Data Types in C#**  
Memory allocation in C# depends on whether the type is a **value type** or **reference type**.

---

## **📌 1. Value Types (Stored on Stack or Inside Objects in Heap)**
Value types are stored **directly in memory** where they are declared (either on the stack or inside heap-allocated objects).  

| **Data Type**  | **Size (bytes)** | **Notes** |
|---------------|----------------|-----------|
| `bool`       | 1              | Can be padded when inside structs |
| `byte`       | 1              | Unsigned 8-bit integer (0 to 255) |
| `sbyte`      | 1              | Signed 8-bit integer (-128 to 127) |
| `char`       | 2              | UTF-16 character |
| `short`      | 2              | Signed 16-bit integer (-32,768 to 32,767) |
| `ushort`     | 2              | Unsigned 16-bit integer (0 to 65,535) |
| `int`        | 4              | Signed 32-bit integer |
| `uint`       | 4              | Unsigned 32-bit integer |
| `float`      | 4              | 32-bit floating-point number (Single) |
| `long`       | 8              | Signed 64-bit integer |
| `ulong`      | 8              | Unsigned 64-bit integer |
| `double`     | 8              | 64-bit floating-point number |
| `decimal`    | 16             | 128-bit high-precision decimal |
| `nint`       | 4 (x86) / 8 (x64) | Native-sized integer |
| `nuint`      | 4 (x86) / 8 (x64) | Native-sized unsigned integer |

📌 **Note:**  
- **Padding & Alignment:** When placed in a struct, additional bytes may be added for alignment.
- **Stack Storage:** Value types are stored **on the stack** unless they are part of an object allocated on the heap.

---

## **📌 2. Reference Types (Stored on Heap, Reference Stored on Stack)**
Reference types store a **reference (pointer) on the stack**, but the actual object **lives in the heap**.

| **Data Type**   | **Memory Size (Heap)**   | **Notes** |
|---------------|----------------|-----------|
| `string`     | 20 + (2 × length) | Immutable, each character is 2 bytes (UTF-16) |
| `object`     | 16 bytes          | Base type for all classes |
| `dynamic`    | Depends on value  | Similar to `object` |
| `class`      | 16 + field sizes  | Contains overhead for object management |
| `array`      | 16 + (Element Size × Count) | Includes array metadata |
| `interface`  | 8 bytes (x64)     | Reference to an implementation |
| `delegate`   | 24+ bytes         | Stores method reference, target, and invocation list |

📌 **Note:**  
- Reference types **always** have a **minimum** size of **16 bytes** for object overhead in .NET.
- Garbage Collector (GC) manages memory allocation and deallocation for reference types.

---

## **📌 3. Special Cases**
### **✔ Structs (Value Type, but Can be in Heap)**
- **Structs** are value types, but when stored inside a class or an array, they are placed **inside the heap**.
- Memory is allocated based on field sizes and alignment.

Example:
```csharp
struct MyStruct
{
    int x;  // 4 bytes
    double y;  // 8 bytes
}
```
**Total Size = 16 bytes** (due to padding for alignment).

### **✔ Nullable Types (`Nullable<T>`)**
- `Nullable<T>` adds **1 extra byte** to store whether the value is `null` or not.

```csharp
int? x;  // Uses 5 bytes (4 for int + 1 for nullable flag)
```

### **✔ Generic Types (`List<T>`, `Dictionary<K,V>`)**
- Generic collections like `List<int>` store elements **inside an array** (heap-allocated).
- Only reference to the collection is stored on the **stack**.

---

## **📌 Memory Comparison Table**
| **Type**       | **Stack / Heap** | **Size (Bytes)** |
|--------------|--------------|--------------|
| `int`        | Stack        | 4 |
| `double`     | Stack        | 8 |
| `bool`       | Stack        | 1 (aligned to 4) |
| `object`     | Heap (Reference on Stack) | 16 |
| `string`     | Heap (Reference on Stack) | 20 + 2 × Length |
| `int[]`      | Heap (Reference on Stack) | 16 + 4 × Length |
| `List<int>`  | Heap (Reference on Stack) | 16 + Capacity × 4 |
| `Dictionary<string, int>` | Heap (Reference on Stack) | Depends on size, hashing overhead |

---

## **📌 Example of Memory Allocation in Action**
```csharp
int x = 10;              // Stored on Stack (4 bytes)
int[] arr = new int[5];  // arr (reference) on Stack, array (heap) = 16 + (5 × 4) = 36 bytes
string name = "Hello";   // name (reference) on Stack, object (heap) = 20 + (5 × 2) = 30 bytes
var list = new List<int>();  // list (reference) on Stack, object (heap) ≈ 16 bytes initially
```

---

## **🔹 Key Takeaways**
✅ **Value types** are stored **on the stack** (unless inside a heap-allocated object).  
✅ **Reference types** store their reference on the **stack**, but the actual object goes **to the heap**.  
✅ **Arrays and Strings** have an **overhead of ~16 bytes** in the heap.  
✅ **Nullable types add an extra byte** for the `null` flag.  
✅ **Garbage Collector (GC)** manages heap allocations for reference types.  
