# **Asynchronous Programming in C# and Task Methods**

Asynchronous programming in C# allows the application to execute long-running operations without blocking the main thread, improving responsiveness and efficiency. The `async` and `await` keywords are used to write asynchronous code using the **Task-based Asynchronous Pattern (TAP)**.

---

## **Key Components of Asynchronous Programming**
1. **`async` keyword** → Marks a method as asynchronous.
2. **`await` keyword** → Suspends execution until the awaited task completes.
3. **`Task` and `Task<T>`** → Represents an asynchronous operation that may return a result.
4. **`ValueTask<T>`** → Optimized alternative to `Task<T>` for high-performance scenarios.

---

## **Real-time Example: Fetching Data from an API**
This example demonstrates calling an external API asynchronously to avoid blocking the main thread.

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        Console.WriteLine("Fetching user details...");
        string userData = await FetchUserDataAsync();
        Console.WriteLine($"User Data: {userData}");
    }

    static async Task<string> FetchUserDataAsync()
    {
        using HttpClient client = new HttpClient();
        HttpResponseMessage response = await client.GetAsync("https://jsonplaceholder.typicode.com/users/1");
        response.EnsureSuccessStatusCode();
        return await response.Content.ReadAsStringAsync();
    }
}
```
### **How it Works?**
1. `FetchUserDataAsync()` calls an external API using `HttpClient.GetAsync()`.
2. `await` pauses execution until the response is received.
3. The `Main` method continues only after data is fetched.

---

## **Common Task Methods in C#**
`Task` and `Task<T>` provide several useful methods for handling asynchronous operations.

### **1. `Task.Run(Action)`**
Runs a method asynchronously on the thread pool.
```csharp
await Task.Run(() => Console.WriteLine("Running in background"));
```
🔹 **Use case:** Running CPU-intensive operations in the background.

---

### **2. `Task.Delay(TimeSpan / int)`**
Pauses execution asynchronously for a specified time.
```csharp
await Task.Delay(2000); // Delays for 2 seconds
Console.WriteLine("Waited 2 seconds");
```
🔹 **Use case:** Retrying operations, scheduling, throttling.

---

### **3. `Task.WhenAll(Task[])`**
Waits for multiple tasks to complete.
```csharp
Task task1 = Task.Delay(1000);
Task task2 = Task.Delay(2000);
await Task.WhenAll(task1, task2);
Console.WriteLine("Both tasks completed");
```
🔹 **Use case:** Running independent tasks in parallel.

---

### **4. `Task.WhenAny(Task[])`**
Waits for the first task to complete.
```csharp
Task<int> task1 = Task.Run(async () => { await Task.Delay(3000); return 1; });
Task<int> task2 = Task.Run(async () => { await Task.Delay(2000); return 2; });

Task<int> firstCompleted = await Task.WhenAny(task1, task2);
Console.WriteLine($"First completed task result: {firstCompleted.Result}");
```
🔹 **Use case:** Handling scenarios where the first available result is needed.

---

### **5. `Task.ContinueWith(Func<Task, T>)`**
Executes a continuation task after another task completes.
```csharp
Task task = Task.Run(() => Console.WriteLine("Main Task"))
                .ContinueWith(t => Console.WriteLine("Continuation Task"));
await task;
```
🔹 **Use case:** Running follow-up tasks after completion.

---

### **6. `Task.FromResult(T)`**
Creates a completed task with a result.
```csharp
Task<int> task = Task.FromResult(42);
int result = await task;
Console.WriteLine($"Result: {result}");
```
🔹 **Use case:** Returning precomputed values asynchronously.

---

### **7. `Task.CompletedTask`**
Represents a completed task with no result.
```csharp
async Task DoSomethingAsync()
{
    Console.WriteLine("Doing work...");
    await Task.CompletedTask;
}
```
🔹 **Use case:** Methods returning `Task` but performing no real asynchronous work.

---

### **8. `Task.Factory.StartNew(Action)`**
Creates and starts a new task.
```csharp
Task task = Task.Factory.StartNew(() => Console.WriteLine("Started with Task.Factory"));
await task;
```
🔹 **Use case:** More configurable than `Task.Run()`, useful for custom task scheduling.

---

### **9. `Task.Yield()`**
Forces the method to yield control back to the caller immediately.
```csharp
await Task.Yield();
Console.WriteLine("Yielded control back to caller");
```
🔹 **Use case:** Preventing UI thread blocking in UI applications.

---

### **10. `Task.Wait()` and `Task.Result`**
⚠️ **Avoid using these in async code! They block the thread.**
```csharp
Task<int> task = Task.Run(() => 10);
task.Wait(); // Blocks until task completes
Console.WriteLine(task.Result); // Blocks and gets the result
```
🔹 **Use case:** Only in synchronous scenarios where blocking is acceptable.

---

## **Example: Combining Multiple Task Methods**
```csharp
using System;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        Task<int> task1 = Task.Run(async () => { await Task.Delay(2000); return 10; });
        Task<int> task2 = Task.FromResult(20);

        Task<int> firstFinished = await Task.WhenAny(task1, task2);
        int result = firstFinished.Result;

        Console.WriteLine($"First completed task result: {result}");
    }
}
```

---

## **Summary of Task Methods**
| Method | Description | Use Case |
|--------|------------|----------|
| `Task.Run()` | Runs code asynchronously on the thread pool | CPU-bound operations |
| `Task.Delay()` | Delays execution asynchronously | Retrying, throttling |
| `Task.WhenAll()` | Waits for all tasks to complete | Running multiple independent tasks |
| `Task.WhenAny()` | Waits for the first task to complete | Handling first available result |
| `Task.ContinueWith()` | Runs a task after another completes | Chaining tasks |
| `Task.FromResult()` | Returns a completed task with a value | Returning precomputed data asynchronously |
| `Task.CompletedTask` | Represents a completed task | Methods returning `Task` without real async work |
| `Task.Factory.StartNew()` | Creates and starts a new task | Custom task scheduling |
| `Task.Yield()` | Yields control to prevent blocking | UI thread responsiveness |
| `Task.Wait()` | Blocks execution until the task completes | Only use in synchronous code |
| `Task.Result` | Blocks execution and gets the result | Avoid in async scenarios |

---

## **Best Practices**
✔ **Use `await` instead of `.Wait()` or `.Result` to avoid deadlocks.**  
✔ **Use `Task.Run()` for CPU-bound tasks but avoid it for async I/O operations.**  
✔ **Use `Task.WhenAll()` for parallel execution of multiple tasks.**  
✔ **Use `Task.Delay()` instead of `Thread.Sleep()` for non-blocking delays.**  

Asynchronous programming in C# makes applications more scalable and responsive, especially when dealing with I/O-bound operations like database queries, API calls, and file operations.

---

In an **async program** in C#, the **Task-based Asynchronous Pattern (TAP)** is used. The mechanism that informs when an `async` operation completes and returns a response is **a combination of the Task and the await mechanism**.

### **Who Notifies When an Async Task Completes?**
1. **The Task Itself (`Task` or `Task<TResult>`)**  
   - The `Task` object represents an ongoing operation and knows when it has completed.
   
2. **The `await` Keyword**  
   - The `await` keyword **pauses execution** of the method until the awaited task completes.
   - Once the task finishes, the execution **resumes** from where it left off.

3. **The Task Scheduler (Thread Pool)**  
   - The **Task Scheduler** runs and monitors tasks using worker threads from the **Thread Pool**.
   - It ensures the `Task` continues executing on the appropriate thread once completed.

---

### **Example of Async Execution Notification**
```csharp
using System;
using System.Threading.Tasks;

class Program
{
    static async Task<string> GetDataAsync()
    {
        Console.WriteLine("Fetching data...");
        await Task.Delay(2000); // Simulating an async operation
        return "Data received!";
    }

    static async Task Main()
    {
        Console.WriteLine("Request sent.");
        string result = await GetDataAsync(); // Task notifies when done
        Console.WriteLine(result); // This runs only after GetDataAsync completes
    }
}
```
### **Flow of Execution:**
1. `Main()` calls `GetDataAsync()`.
2. `await Task.Delay(2000);` **pauses execution** and **returns control** to the caller (`Main`).
3. Once the `Task.Delay` completes, the **Task Scheduler** resumes execution of `GetDataAsync()`.
4. The method returns `"Data received!"`, and `Main()` prints it.

---

### **Alternative: Using `ContinueWith()` (Without `await`)**
Instead of `await`, we can use `.ContinueWith()` to manually specify what should happen when a task completes.

```csharp
static Task<string> GetDataAsync()
{
    return Task.Run(() =>
    {
        Task.Delay(2000).Wait(); // Simulating delay
        return "Data received!";
    });
}

static void Main()
{
    Console.WriteLine("Request sent.");
    GetDataAsync().ContinueWith(task => Console.WriteLine(task.Result)); // Notification after completion
    Console.ReadLine(); // Keep console open
}
```
💡 **Note:** `.ContinueWith()` is more complex and less recommended than `await` for readability.

---

### **Summary**
- The **Task itself** tracks when an async operation completes.
- The **await keyword** pauses execution and resumes it once the Task completes.
- The **Task Scheduler (Thread Pool)** ensures execution continues on the right thread.
- **Event-driven execution** ensures the caller gets notified when the async method completes.

🚀 **In most cases, `await` is the best way to handle async execution in C#.**
