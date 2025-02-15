## **Blazor Overview**  

Blazor is a modern **UI framework** developed by Microsoft that allows developers to build interactive **web applications using C# and .NET instead of JavaScript**. It is part of **ASP.NET Core** and uses **Razor components** to create reusable UI elements.

---

## **1. Key Features of Blazor**  
✅ **Single Language (C#)** – Write both client-side and server-side code in C#.  
✅ **Component-Based Architecture** – Uses reusable components with Razor syntax (`.razor`).  
✅ **WebAssembly Support** – Runs directly in the browser without JavaScript dependencies.  
✅ **Full Stack .NET Development** – No need for JavaScript frameworks like React or Angular.  
✅ **Dependency Injection (DI)** – Supports built-in DI similar to ASP.NET Core.  
✅ **JavaScript Interop** – Calls JavaScript functions from C# and vice versa.  
✅ **Routing** – Built-in routing system similar to ASP.NET Core MVC.  
✅ **State Management** – Uses cascading parameters, session storage, and state containers.  

---

## **2. Blazor Hosting Models**  
Blazor has **two primary hosting models**, each with different architectures and use cases:

### **🟢 Blazor Server** (Interactive UI via SignalR)  
- **Runs on the server**, while UI updates are sent via **SignalR WebSockets**.  
- **Thin client, heavy server** – Good for apps requiring real-time updates.  
- **Fast initial load**, but **requires constant server connection**.  
- **Ideal for internal web apps, dashboards, and admin panels**.  

**Example Architecture:**  
```
Browser <----> SignalR WebSocket <----> Blazor Server <----> Database
```

**Blazor Server Code Example:**  
```csharp
@page "/counter"
<h3>Counter</h3>
<p>Count: @count</p>
<button @onclick="Increment">Click me</button>

@code {
    private int count = 0;
    private void Increment() => count++;
}
```

---

### **🔵 Blazor WebAssembly (WASM)** (Client-Side Execution)  
- **Runs in the browser using WebAssembly** (No need for a .NET server).  
- **Everything is downloaded to the client** (initial load can be slow).  
- **Works offline** after the first load.  
- **Best for PWA, static sites, and modern SPAs**.  

**Example Architecture:**  
```
Browser <----> WebAssembly <----> API (Optional) <----> Database
```

**Blazor WebAssembly Code Example:**  
```csharp
@page "/counter"
<h3>Counter</h3>
<p>Count: @count</p>
<button @onclick="Increment">Click me</button>

@code {
    private int count = 0;
    private void Increment() => count++;
}
```

---

## **3. Blazor Components & Syntax**  
Blazor applications are built using **components** (`.razor` files), which are reusable UI elements.

### **🟢 Basic Component Example**  
```razor
<h3>Hello, @Name!</h3>
<p>Current Time: @DateTime.Now</p>

@code {
    private string Name = "Blazor";
}
```

### **🟢 Passing Parameters to Components**  
```razor
<MyComponent Title="Blazor Interview Prep" />
```
```razor
@code {
    [Parameter] public string Title { get; set; }
}
```

---

## **4. Blazor Routing**
Blazor provides **built-in routing** using the `@page` directive.

```razor
@page "/home"
<h3>Welcome to Home Page</h3>
```
Navigating to `/home` in the browser will load this component.

---

## **5. Blazor Dependency Injection**
Blazor supports **built-in DI**, allowing services to be injected into components.

**Register a Service (`Program.cs`):**  
```csharp
builder.Services.AddScoped<WeatherService>();
```

**Inject and Use in Component:**  
```razor
@inject WeatherService WeatherService

<p>Temperature: @WeatherService.GetTemperature()</p>
```

---

## **6. Blazor JavaScript Interop**
Blazor can **call JavaScript functions from C# and vice versa**.

```js
// JavaScript function
function showAlert(message) {
    alert(message);
}
```

```csharp
@inject IJSRuntime JS

<button @onclick="ShowMessage">Click Me</button>

@code {
    private async Task ShowMessage() {
        await JS.InvokeVoidAsync("showAlert", "Hello from Blazor!");
    }
}
```

---

## **7. Blazor Authentication & Authorization**  
Blazor supports authentication using **OAuth, OpenID Connect, JWT, and IdentityServer**.

**Protecting Routes with `[Authorize]`:**  
```razor
@attribute [Authorize]
<h3>Protected Page</h3>
<p>Only authenticated users can see this.</p>
```

---

## **8. Blazor State Management**
Blazor provides multiple ways to **maintain state** across components and sessions.

- **Cascading Parameters**
- **Session Storage / Local Storage**
- **State Container (Singleton Services)**
- **Persistent Storage (Database, API)**

---

## **9. Blazor Performance Optimization**
To improve Blazor performance:
✅ **Use `ShouldRender()`** to prevent unnecessary re-renders.  
✅ **Lazy Load Assemblies** in Blazor WebAssembly.  
✅ **Use Asynchronous Methods** for data fetching (`async/await`).  
✅ **Minimize JavaScript Interop Calls** (Only call JavaScript when necessary).  
✅ **Use Virtualization** for large lists (`<Virtualize>` component).  

---

## **10. Blazor Real-Time Updates (SignalR)**
Blazor Server can use **SignalR** for real-time updates.

```csharp
public class ChatHub : Hub
{
    public async Task SendMessage(string user, string message)
    {
        await Clients.All.SendAsync("ReceiveMessage", user, message);
    }
}
```

---

## **Common Blazor Interview Questions**
1️⃣ What are the differences between **Blazor Server** and **Blazor WebAssembly**?  
2️⃣ How does Blazor handle **component lifecycle** events?  
3️⃣ How can you call **JavaScript functions** in Blazor?  
4️⃣ What are **EventCallback** and **RenderFragment** used for?  
5️⃣ How do you manage **state** in Blazor applications?  
6️⃣ How does **dependency injection (DI)** work in Blazor?  
7️⃣ What are **Cascading Parameters**, and when should you use them?  
8️⃣ How can you implement **authentication & authorization** in Blazor?  
9️⃣ How does Blazor handle **routing and navigation**?  
🔟 What are some **performance optimizations** for Blazor apps?  

---

## **When to Use Blazor?**
✅ **Ideal for .NET Developers** – Reuse existing C# skills instead of learning JavaScript.  
✅ **Enterprise Web Applications** – Works well with ASP.NET Core APIs.  
✅ **Real-Time Applications** – Blazor Server + SignalR for live updates.  
✅ **Progressive Web Apps (PWA)** – Blazor WebAssembly allows offline support.  
✅ **Modernizing Legacy .NET Apps** – Replace WebForms or MVC with Blazor components.  

---

### **Summary Table: Blazor Server vs. Blazor WebAssembly**
| Feature            | Blazor Server   | Blazor WebAssembly |
|--------------------|----------------|--------------------|
| Hosting           | Runs on Server  | Runs in Browser   |
| Execution Model   | Uses SignalR    | Uses WebAssembly  |
| Performance      | Faster load time | Slower initial load |
| Offline Support  | ❌ No            | ✅ Yes            |
| Resource Usage   | Higher server load | Higher client load |
| Security        | More secure     | Less secure (code runs on client) |

---

### **Final Thoughts**
Blazor is a powerful **.NET UI framework** that enables **full-stack development using C#**. Whether using **Blazor Server** for real-time apps or **Blazor WebAssembly** for client-side execution, it provides a **modern and efficient** approach to building web applications **without relying on JavaScript frameworks**.

---
---
# **Blazor Component Lifecycle**  

Blazor components go through a well-defined lifecycle with specific lifecycle methods that allow developers to execute logic at different stages of the component's creation, rendering, and disposal.

---

## **Blazor Lifecycle Methods**
### **1. Component Initialization Phase**
These methods are called when the component is first initialized.  

#### **`OnInitialized` / `OnInitializedAsync`**  
- Called once when the component is initialized (before rendering).  
- Used to set up initial state or fetch data asynchronously.  
- `OnInitializedAsync` is used when asynchronous operations are required.

```razor
@code {
    protected override void OnInitialized()
    {
        Console.WriteLine("Component Initialized!");
    }

    protected override async Task OnInitializedAsync()
    {
        await Task.Delay(1000); // Simulate async operation
        Console.WriteLine("Component Initialized Asynchronously!");
    }
}
```

---

### **2. Parameter Processing Phase**  
These methods are called when parameters are received from the parent component.

#### **`OnParametersSet` / `OnParametersSetAsync`**  
- Invoked every time parameter values change.  
- `OnParametersSetAsync` is used for async operations.

```razor
@code {
    [Parameter] public string Name { get; set; }

    protected override void OnParametersSet()
    {
        Console.WriteLine($"Parameter updated: {Name}");
    }

    protected override async Task OnParametersSetAsync()
    {
        await Task.Delay(500);
        Console.WriteLine("Parameters set asynchronously.");
    }
}
```

---

### **3. Rendering Phase**  
These methods are called when the component needs to render.

#### **`ShouldRender`**  
- Determines if the component should render.  
- Returns `true` (default) to allow rendering, `false` to prevent unnecessary re-renders.

```razor
@code {
    private bool shouldRender = true;

    protected override bool ShouldRender()
    {
        return shouldRender;
    }
}
```

#### **`OnAfterRender` / `OnAfterRenderAsync`**  
- Called after the component has rendered.  
- `OnAfterRenderAsync` is used for async tasks (e.g., JavaScript interop).  
- Runs only after the **first** render unless manually triggered.

```razor
@code {
    protected override void OnAfterRender(bool firstRender)
    {
        if (firstRender)
        {
            Console.WriteLine("First render completed!");
        }
    }

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender)
        {
            await Task.Delay(500);
            Console.WriteLine("First render async completed!");
        }
    }
}
```

---

### **4. Component Disposal Phase**  
Called when the component is being removed.

#### **`Dispose` / `DisposeAsync`**  
- Used for cleaning up resources (e.g., event handlers, timers).  
- Implements `IDisposable` or `IAsyncDisposable` interface.

```razor
@code {
    private Timer _timer;

    protected override void OnInitialized()
    {
        _timer = new Timer(state => Console.WriteLine("Timer Tick"), null, 0, 1000);
    }

    public void Dispose()
    {
        _timer?.Dispose();
        Console.WriteLine("Component Disposed!");
    }
}
```

---

## **Lifecycle Execution Order**
1. **Component Initialization**
   - `OnInitialized` → `OnInitializedAsync`
2. **Parameter Updates**
   - `OnParametersSet` → `OnParametersSetAsync`
3. **Rendering**
   - `ShouldRender`
   - `OnAfterRender` → `OnAfterRenderAsync`
4. **Component Destruction**
   - `Dispose` → `DisposeAsync`

---

### **Diagram Representation**  
```
Component Created → OnInitialized / OnInitializedAsync  
    ↓  
Parameters Set → OnParametersSet / OnParametersSetAsync  
    ↓  
ShouldRender (if true) → UI Render  
    ↓  
OnAfterRender / OnAfterRenderAsync  
    ↓  
Component Disposed → Dispose / DisposeAsync  
```
