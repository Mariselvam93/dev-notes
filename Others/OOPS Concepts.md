# Object-Oriented Programming (OOP)
---
Object-Oriented Programming (OOP) in C# consists of four main principles:

1. **Encapsulation**  
2. **Abstraction**  
3. **Inheritance**  
4. **Polymorphism**  

Let's go through each concept with real-time examples in C#.  

---

### **1. Encapsulation** (Data Hiding)
Encapsulation is the practice of bundling data (variables) and methods that operate on the data into a single unit (class). It also restricts direct access to some of the object's components, which is achieved using access modifiers (`private`, `protected`, `public`, etc.).

#### **Real-time Example: ATM Machine**
An ATM encapsulates the user’s bank balance and only allows limited operations (withdraw, deposit, check balance) without exposing the internal data.

```csharp
public class BankAccount
{
    private double balance; // Private field, not accessible directly

    public BankAccount(double initialBalance)
    {
        balance = initialBalance;
    }

    public double GetBalance() // Encapsulation: Controlled access to data
    {
        return balance;
    }

    public void Deposit(double amount)
    {
        if (amount > 0)
            balance += amount;
    }

    public void Withdraw(double amount)
    {
        if (amount > 0 && amount <= balance)
            balance -= amount;
    }
}

class Program
{
    static void Main()
    {
        BankAccount account = new BankAccount(5000);
        account.Deposit(1500);
        account.Withdraw(2000);
        Console.WriteLine("Current Balance: " + account.GetBalance()); // Output: 4500
    }
}
```

---

### **2. Abstraction** (Hiding Implementation Details)
Abstraction is the process of exposing only the necessary details while hiding the implementation logic.

#### **Real-time Example: Car**
When you drive a car, you use the steering wheel, accelerator, and brake without knowing the internal engine mechanisms.

```csharp
public abstract class Car
{
    public abstract void Start(); // Abstract method, must be implemented by derived classes
    public abstract void Stop();

    public void FuelUp()
    {
        Console.WriteLine("Filling fuel...");
    }
}

public class Tesla : Car
{
    public override void Start()
    {
        Console.WriteLine("Tesla is starting silently...");
    }

    public override void Stop()
    {
        Console.WriteLine("Tesla is stopping...");
    }
}

class Program
{
    static void Main()
    {
        Car myCar = new Tesla();
        myCar.Start();  // Abstract method implementation
        myCar.FuelUp(); // Concrete method
        myCar.Stop();
    }
}
```

---

### **3. Inheritance** (Code Reusability)
Inheritance allows a class (child) to derive properties and methods from another class (parent). It promotes reusability.

#### **Real-time Example: Employee Hierarchy**
An `Employee` base class can be inherited by `Manager` and `Developer` classes, avoiding code duplication.

```csharp
public class Employee
{
    public string Name { get; set; }
    public int EmployeeId { get; set; }

    public void Work()
    {
        Console.WriteLine(Name + " is working.");
    }
}

public class Manager : Employee
{
    public void Manage()
    {
        Console.WriteLine(Name + " is managing the team.");
    }
}

public class Developer : Employee
{
    public void Code()
    {
        Console.WriteLine(Name + " is writing code.");
    }
}

class Program
{
    static void Main()
    {
        Manager manager = new Manager() { Name = "Alice", EmployeeId = 101 };
        Developer developer = new Developer() { Name = "Bob", EmployeeId = 102 };

        manager.Work();
        manager.Manage();

        developer.Work();
        developer.Code();
    }
}
```

---

### **4. Polymorphism** (Multiple Forms)
Polymorphism allows a method to have different behaviors based on the object that calls it.

#### **Real-time Example: Payment System**
Different payment methods (Credit Card, PayPal, Google Pay) have different implementations of the `MakePayment` method.

```csharp
public class Payment
{
    public virtual void MakePayment(double amount)
    {
        Console.WriteLine("Payment of $" + amount + " made.");
    }
}

public class CreditCardPayment : Payment
{
    public override void MakePayment(double amount)
    {
        Console.WriteLine("Payment of $" + amount + " made using Credit Card.");
    }
}

public class PayPalPayment : Payment
{
    public override void MakePayment(double amount)
    {
        Console.WriteLine("Payment of $" + amount + " made using PayPal.");
    }
}

class Program
{
    static void Main()
    {
        Payment payment;

        payment = new CreditCardPayment();
        payment.MakePayment(100); // Output: Payment of $100 made using Credit Card.

        payment = new PayPalPayment();
        payment.MakePayment(200); // Output: Payment of $200 made using PayPal.
    }
}
```

---

## **Summary**
| OOP Concept   | Description | Real-time Example |  
|--------------|-------------|----------------|  
| **Encapsulation** | Hides internal data and provides controlled access. | ATM Machine (Balance cannot be accessed directly) |  
| **Abstraction** | Hides implementation details and only exposes essential features. | Car (You don’t need to know how the engine works) |  
| **Inheritance** | Child class inherits properties and behavior from the parent class. | Employee Hierarchy (Manager and Developer inherit from Employee) |  
| **Polymorphism** | One interface, multiple implementations. | Payment System (Different payment methods like Credit Card, PayPal) |
