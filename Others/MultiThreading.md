# **Multithreading in C# with Practical Example**

## **Introduction to Multithreading in C#**

### **What is Multithreading?**
Multithreading is a programming technique where multiple threads run concurrently to execute different tasks. It allows applications to perform **parallel processing**, improving efficiency and performance.

### **Key Benefits of Multithreading**
- **Improves performance** by utilizing CPU cores efficiently.
- **Reduces execution time** by running tasks concurrently.
- **Prevents UI freezing** in applications by running heavy tasks in background threads.
- **Enhances responsiveness** in real-time applications.

## **Managing Multithreading in C#**
C# provides several ways to manage and synchronize threads to avoid issues like race conditions and deadlocks. Common synchronization mechanisms include:

1. **`SemaphoreSlim`** – Limits the number of concurrent threads.
2. **`Mutex`** – Ensures only one thread accesses a resource at a time across multiple processes.
3. **`lock`** – Ensures that only one thread executes a critical section of code.
4. **`Monitor`** – Provides more advanced thread synchronization.
5. **`AutoResetEvent` & `ManualResetEvent`** – Used for thread signaling.

## **Practical Example: Processing Multiple Files Concurrently**

### **Scenario**
We have multiple files in a directory that need to be processed (e.g., reading content and writing a modified version). To efficiently handle this task:
- We use **multithreading** to process multiple files concurrently.
- We limit concurrent threads using **`SemaphoreSlim`** to prevent resource overuse.
- We ensure safe logging using **`lock`**.

### **Implementation**
```csharp
using System;
using System.IO;
using System.Threading;

class Program
{
    private static readonly SemaphoreSlim semaphore = new SemaphoreSlim(2); // Allow 2 threads at a time
    private static readonly object _lockObj = new object(); // Lock for thread-safe logging

    static void ProcessFile(string filePath)
    {
        semaphore.Wait(); // Wait for available slot

        try
        {
            Console.WriteLine($"Thread {Thread.CurrentThread.ManagedThreadId} processing {filePath}");

            // Simulating file read operation
            string content = File.ReadAllText(filePath);
            string modifiedContent = content.ToUpper(); // Modify content (e.g., convert to uppercase)

            // Writing processed content back to a new file
            string newFilePath = $"{filePath}.processed";
            File.WriteAllText(newFilePath, modifiedContent);

            // Thread-safe logging
            lock (_lockObj)
            {
                File.AppendAllText("processing_log.txt", $"Processed: {filePath} by Thread {Thread.CurrentThread.ManagedThreadId}\n");
            }

            Console.WriteLine($"Thread {Thread.CurrentThread.ManagedThreadId} finished processing {filePath}");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error processing {filePath}: {ex.Message}");
        }
        finally
        {
            semaphore.Release(); // Release slot for next thread
        }
    }

    static void Main()
    {
        string[] files = Directory.GetFiles("C:\\Temp\\FilesToProcess"); // Directory containing files
        Thread[] threads = new Thread[files.Length];

        for (int i = 0; i < files.Length; i++)
        {
            threads[i] = new Thread(() => ProcessFile(files[i]));
            threads[i].Start();
        }

        foreach (var thread in threads)
        {
            thread.Join(); // Wait for all threads to finish
        }

        Console.WriteLine("All files processed successfully.");
    }
}
```

## **Explanation**
1. **Using `SemaphoreSlim(2)`**  
   - Limits the number of concurrent file-processing threads to **2** at a time to prevent excessive resource usage.

2. **Using `lock` for Logging**  
   - Ensures **only one thread writes to the log file at a time**, avoiding race conditions.

3. **Thread Creation and Execution**  
   - Creates multiple `Thread` instances to process each file concurrently.
   - Each thread runs `ProcessFile(filePath)`, which reads, modifies, and writes the content.

4. **Thread Synchronization with `Join`**  
   - Ensures the `Main()` method waits for all threads to finish before exiting.

## **Example Output**
```
Thread 3 processing C:\Temp\FilesToProcess\file1.txt
Thread 4 processing C:\Temp\FilesToProcess\file2.txt
Thread 3 finished processing C:\Temp\FilesToProcess\file1.txt
Thread 4 finished processing C:\Temp\FilesToProcess\file2.txt
Thread 5 processing C:\Temp\FilesToProcess\file3.txt
...
All files processed successfully.
```
- **Only two threads** work at a time due to `SemaphoreSlim(2)`.
- **Log file (`processing_log.txt`) contains:**
  ```
  Processed: C:\Temp\FilesToProcess\file1.txt by Thread 3
  Processed: C:\Temp\FilesToProcess\file2.txt by Thread 4
  ...
  ```

## **Key Takeaways**
- **Efficiently processes multiple files** using **multithreading**.
- **Avoids excessive thread creation** using **`SemaphoreSlim`**.
- **Ensures safe file logging** using **`lock`**.
- **Waits for all threads to complete** using **`Join`**.



