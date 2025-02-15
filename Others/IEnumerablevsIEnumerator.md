### **IEnumerable vs IEnumerator (Brief Explanation)**  

#### **1. `IEnumerable<T>` (Collection Interface)**
- Represents a collection that can be iterated.
- Contains only **one method**: `GetEnumerator()`, which returns an `IEnumerator<T>`.
- Used in **foreach loops** to iterate over collections.

**Example:**
```csharp
public class MyCollection : IEnumerable<int>
{
    private List<int> _items = new() { 1, 2, 3 };

    public IEnumerator<int> GetEnumerator() => _items.GetEnumerator();

    IEnumerator IEnumerable.GetEnumerator() => GetEnumerator();
}
```

#### **2. `IEnumerator<T>` (Iterator Interface)**
- Provides **controlled** iteration over a collection.
- Has `MoveNext()`, `Current`, and `Reset()` methods.
- Used when you need **manual iteration**.

**Example:**
```csharp
public class MyEnumerator : IEnumerator<int>
{
    private int[] _items;
    private int _index = -1;

    public MyEnumerator(int[] items) => _items = items;

    public int Current => _items[_index];

    public bool MoveNext() => ++_index < _items.Length;

    public void Reset() => _index = -1;

    public void Dispose() { }
}
```

---

### **Key Differences**
| Feature          | `IEnumerable<T>` | `IEnumerator<T>` |
|----------------|----------------|----------------|
| Purpose | Represents a collection | Controls iteration |
| Key Method | `GetEnumerator()` | `MoveNext()`, `Current`, `Reset()` |
| Used in | `foreach` loops | Manual iteration |
| Stores State | No | Yes |

✅ **Use `IEnumerable<T>`** when you need a collection to be iterated.  
✅ **Use `IEnumerator<T>`** when you need full control over iteration.