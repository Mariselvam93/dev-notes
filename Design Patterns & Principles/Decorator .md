## **Decorator Design Pattern**
The **Decorator Pattern** is a **structural design pattern** that allows behavior to be added to individual objects, dynamically, without modifying their code. It follows the **Open-Closed Principle**, which states that classes should be open for extension but closed for modification.

### **Key Concepts:**
1. **Component (Interface or Abstract Class)** – Defines the common interface for both concrete components and decorators.
2. **Concrete Component** – The base implementation that can be decorated.
3. **Decorator (Abstract Class)** – Inherits from the component and contains a reference to a component instance.
4. **Concrete Decorators** – Extend the behavior of the base component.

---

## **Example in C#**
### **Basic Coffee Example**
Imagine a coffee shop where a base coffee can have different add-ons (decorators) like Milk, Sugar, and Caramel.

### **Step 1: Define the Component Interface**
```csharp
public interface ICoffee
{
    string GetDescription();
    double GetCost();
}
```

### **Step 2: Implement the Concrete Component**
```csharp
public class SimpleCoffee : ICoffee
{
    public string GetDescription() => "Simple Coffee";
    public double GetCost() => 5.0;
}
```

### **Step 3: Create the Decorator Abstract Class**
```csharp
public abstract class CoffeeDecorator : ICoffee
{
    protected ICoffee _coffee;

    public CoffeeDecorator(ICoffee coffee)
    {
        _coffee = coffee;
    }

    public virtual string GetDescription() => _coffee.GetDescription();
    public virtual double GetCost() => _coffee.GetCost();
}
```

### **Step 4: Implement Concrete Decorators**
```csharp
public class MilkDecorator : CoffeeDecorator
{
    public MilkDecorator(ICoffee coffee) : base(coffee) { }

    public override string GetDescription() => _coffee.GetDescription() + ", Milk";
    public override double GetCost() => _coffee.GetCost() + 1.5;
}

public class SugarDecorator : CoffeeDecorator
{
    public SugarDecorator(ICoffee coffee) : base(coffee) { }

    public override string GetDescription() => _coffee.GetDescription() + ", Sugar";
    public override double GetCost() => _coffee.GetCost() + 0.5;
}
```

### **Step 5: Use the Decorators**
```csharp
class Program
{
    static void Main()
    {
        ICoffee coffee = new SimpleCoffee();
        Console.WriteLine($"{coffee.GetDescription()} - ${coffee.GetCost()}");

        coffee = new MilkDecorator(coffee);
        Console.WriteLine($"{coffee.GetDescription()} - ${coffee.GetCost()}");

        coffee = new SugarDecorator(coffee);
        Console.WriteLine($"{coffee.GetDescription()} - ${coffee.GetCost()}");
    }
}
```

### **Output**
```
Simple Coffee - $5.0
Simple Coffee, Milk - $6.5
Simple Coffee, Milk, Sugar - $7.0
```

---

## **Use Cases of Decorator Pattern**
### **1. Enhancing Object Functionality Dynamically**
   - Example: Adding toppings to a pizza or coffee.

### **2. Logging & Monitoring**
   - Example: Wrapping methods to log execution time.
   ```csharp
   public class LoggingDecorator : ICoffee
   {
       private readonly ICoffee _coffee;
       public LoggingDecorator(ICoffee coffee) { _coffee = coffee; }

       public string GetDescription()
       {
           Console.WriteLine("Logging: Getting Description");
           return _coffee.GetDescription();
       }

       public double GetCost()
       {
           Console.WriteLine("Logging: Getting Cost");
           return _coffee.GetCost();
       }
   }
   ```

### **3. Security & Authentication**
   - Example: Adding an authentication check before executing a method.
   ```csharp
   public class AuthDecorator : ICoffee
   {
       private readonly ICoffee _coffee;
       public AuthDecorator(ICoffee coffee) { _coffee = coffee; }

       public string GetDescription()
       {
           if (!UserIsAuthenticated()) throw new UnauthorizedAccessException();
           return _coffee.GetDescription();
       }

       public double GetCost()
       {
           if (!UserIsAuthenticated()) throw new UnauthorizedAccessException();
           return _coffee.GetCost();
       }

       private bool UserIsAuthenticated() => true; // Mock authentication
   }
   ```

### **4. Stream Handling in .NET**
   - The `Stream` class in .NET uses decorators:
   ```csharp
   using (var fileStream = new FileStream("data.txt", FileMode.Open))
   using (var reader = new StreamReader(fileStream)) // Decorator
   {
       Console.WriteLine(reader.ReadToEnd());
   }
   ```

### **5. Data Compression and Encryption**
   - Example: `GZipStream` in .NET.
   ```csharp
   using (FileStream file = File.Create("compressed.gz"))
   using (GZipStream compressor = new GZipStream(file, CompressionMode.Compress)) 
   {
       byte[] data = Encoding.UTF8.GetBytes("Hello, Decorator Pattern!");
       compressor.Write(data, 0, data.Length);
   }
   ```

---

## **When to Use Decorator Pattern**
✔ When you need to extend behavior dynamically.  
✔ When subclassing is not feasible due to class explosion.  
✔ When you want to follow **Open-Closed Principle**.  

🚫 Avoid when object structure gets too complex.  
🚫 Not useful if modifications are frequent and require changes in many places.
