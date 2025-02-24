Here’s a breakdown of HTTP status codes categorized by their types, along with real-world scenarios where they are commonly encountered.

---

## **1xx - Informational Responses**
These indicate that the server has received the request and is processing it.

- **100 Continue**  
  - *Use Case:* A client sends an `Expect: 100-continue` header before sending a large payload to check if the server is ready to accept it.
  
- **101 Switching Protocols**  
  - *Use Case:* A WebSocket handshake where the client requests to switch from HTTP to WebSocket.

- **102 Processing** (WebDAV)  
  - *Use Case:* When a server is performing a long-running operation and informs the client to wait.

---

## **2xx - Success Responses**
These indicate that the request was successfully received, understood, and processed.

- **200 OK**  
  - *Use Case:* A successful API request returning a JSON response.
  - Example: `GET /users/1` returns user details.

- **201 Created**  
  - *Use Case:* When a new resource is successfully created. 
  - Example: `POST /users` creates a new user and returns `201`.

- **202 Accepted**  
  - *Use Case:* When an asynchronous process starts but hasn't completed yet.
  - Example: Submitting a background job.

- **204 No Content**  
  - *Use Case:* When a request is successful but there's no content to return.
  - Example: `DELETE /users/1` where the user is deleted but nothing is returned.

---

## **3xx - Redirection Responses**
These indicate that further action is needed to complete the request.

- **301 Moved Permanently**  
  - *Use Case:* When a resource is permanently moved to a new URL.
  - Example: `http://example.com` redirects to `https://example.com`.

- **302 Found (Temporary Redirect)**  
  - *Use Case:* A temporary redirection.
  - Example: A user is redirected to a login page before accessing a protected resource.

- **304 Not Modified**  
  - *Use Case:* When the requested resource hasn't changed since the last request.
  - Example: A browser caching mechanism where `If-Modified-Since` is used.

---

## **4xx - Client Errors**
These indicate that the request contains bad syntax or cannot be fulfilled.

- **400 Bad Request**  
  - *Use Case:* Invalid request parameters or malformed JSON.
  - Example: `POST /users` with missing required fields.

- **401 Unauthorized**  
  - *Use Case:* Authentication is required but not provided or is invalid.
  - Example: Accessing a protected API without a valid token.

- **403 Forbidden**  
  - *Use Case:* The request is valid but the user lacks permission.
  - Example: Trying to access an admin page without admin rights.

- **404 Not Found**  
  - *Use Case:* The requested resource doesn't exist.
  - Example: `GET /nonexistent-page`.

- **405 Method Not Allowed**  
  - *Use Case:* When a resource doesn't support the HTTP method used.
  - Example: `DELETE /users/1` when DELETE isn't allowed.

- **409 Conflict**  
  - *Use Case:* When a request conflicts with the current state of the resource.
  - Example: Creating a user with an already existing email.

- **413 Payload Too Large**  
  - *Use Case:* When a file upload exceeds the server’s maximum size limit.

- **429 Too Many Requests**  
  - *Use Case:* When a client exceeds the rate limit for an API.
  - Example: A user makes too many login attempts in a short period.

---

## **5xx - Server Errors**
These indicate that the server failed to fulfill a valid request.

- **500 Internal Server Error**  
  - *Use Case:* A generic error when something unexpected happens.
  - Example: A database connection failure.

- **502 Bad Gateway**  
  - *Use Case:* When a proxy or gateway receives an invalid response from the upstream server.
  - Example: NGINX receiving a faulty response from a backend service.

- **503 Service Unavailable**  
  - *Use Case:* When a server is overloaded or down for maintenance.
  - Example: A website temporarily unavailable due to maintenance.

- **504 Gateway Timeout**  
  - *Use Case:* When a server acting as a gateway doesn't get a timely response.
  - Example: A request to an external API that takes too long to respond.

---

# **HTTP Status Codes - Use Cases & Code Examples**

Here are code examples for common HTTP status codes using **ASP.NET Core (.NET 8 Web API)**:

---

## **1. 200 OK - Successful Response**
### **Example:** Fetching a User by ID  
```csharp
[HttpGet("{id}")]
public IActionResult GetUser(int id)
{
    var user = _userService.GetUserById(id);
    if (user == null)
        return NotFound(); // Returns 404 if user doesn't exist

    return Ok(user); // Returns 200 OK with user data
}
```
📌 **Use Case:** `GET /users/1` → Returns user details.

---

## **2. 201 Created - Resource Created**
### **Example:** Creating a New User  
```csharp
[HttpPost]
public IActionResult CreateUser([FromBody] UserDto userDto)
{
    var user = _userService.CreateUser(userDto);
    return CreatedAtAction(nameof(GetUser), new { id = user.Id }, user);
}
```
📌 **Use Case:** `POST /users` → Returns `201 Created` with a location header.

---

## **3. 204 No Content - Successful but No Response**
### **Example:** Deleting a User  
```csharp
[HttpDelete("{id}")]
public IActionResult DeleteUser(int id)
{
    bool deleted = _userService.DeleteUser(id);
    if (!deleted)
        return NotFound(); // 404 if user not found

    return NoContent(); // 204 No Content
}
```
📌 **Use Case:** `DELETE /users/1` → Deletes user but returns no content.

---

## **4. 400 Bad Request - Invalid Input**
### **Example:** Validation Error  
```csharp
[HttpPost]
public IActionResult RegisterUser([FromBody] RegisterDto model)
{
    if (!ModelState.IsValid)
        return BadRequest(ModelState); // 400 Bad Request

    var user = _userService.Register(model);
    return Ok(user);
}
```
📌 **Use Case:** `POST /register` with missing required fields → Returns `400 Bad Request`.

---

## **5. 401 Unauthorized - No Authentication**
### **Example:** Accessing a Protected Resource  
```csharp
[Authorize]
[HttpGet("profile")]
public IActionResult GetProfile()
{
    var userId = User.FindFirst(ClaimTypes.NameIdentifier)?.Value;
    var profile = _userService.GetUserProfile(userId);

    return Ok(profile);
}
```
📌 **Use Case:** `GET /profile` without a valid token → Returns `401 Unauthorized`.

---

## **6. 403 Forbidden - Insufficient Permissions**
### **Example:** Admin-Only Endpoint  
```csharp
[Authorize(Roles = "Admin")]
[HttpGet("admin-dashboard")]
public IActionResult AdminDashboard()
{
    return Ok("Welcome Admin!");
}
```
📌 **Use Case:** `GET /admin-dashboard` as a regular user → Returns `403 Forbidden`.

---

## **7. 404 Not Found - Resource Missing**
### **Example:** Fetching a Non-Existing Resource  
```csharp
[HttpGet("{id}")]
public IActionResult GetProduct(int id)
{
    var product = _productService.GetProductById(id);
    if (product == null)
        return NotFound(new { Message = "Product not found" });

    return Ok(product);
}
```
📌 **Use Case:** `GET /products/999` for a missing product → Returns `404 Not Found`.

---

## **8. 409 Conflict - Duplicate Entry**
### **Example:** Creating a User with an Existing Email  
```csharp
[HttpPost]
public IActionResult Register([FromBody] RegisterDto model)
{
    if (_userService.EmailExists(model.Email))
        return Conflict(new { Message = "Email already registered" });

    var user = _userService.Register(model);
    return CreatedAtAction(nameof(GetUser), new { id = user.Id }, user);
}
```
📌 **Use Case:** `POST /register` with an existing email → Returns `409 Conflict`.

---

## **9. 429 Too Many Requests - Rate Limiting**
### **Example:** API Rate Limit  
```csharp
[HttpGet("limited-resource")]
[ServiceFilter(typeof(RateLimitFilter))] // Custom rate-limiting filter
public IActionResult GetLimitedResource()
{
    return Ok("Request successful");
}
```
📌 **Use Case:** Exceeding request limits → Returns `429 Too Many Requests`.

---

### **10. 500 Internal Server Error**
- **Meaning:** The server encountered an unexpected error.
- **Use Case:** A database failure.

```csharp
try
{
    var user = _userService.GetUserById(id);
    return Ok(user);
}
catch (Exception ex)
{
    _logger.LogError(ex, "Error fetching user");
    return StatusCode(500, "Internal Server Error");
}
```

---

### **11. 501 Not Implemented**
- **Meaning:** The server does not support the request.
- **Use Case:** An unfinished API feature.

```csharp
[HttpGet("upcoming-feature")]
public IActionResult GetUpcomingFeature()
{
    return StatusCode(501, "Feature not implemented"); // 501 Not Implemented
}
```

---

### **12. 502 Bad Gateway**
- **Meaning:** The server received an invalid response from an upstream server.
- **Use Case:** A failing API Gateway.

```csharp
try
{
    var response = await _httpClient.GetAsync("https://third-party-api.com");
    if (!response.IsSuccessStatusCode) return StatusCode(502, "Bad Gateway");
    return Ok(await response.Content.ReadAsStringAsync());
}
catch (HttpRequestException)
{
    return StatusCode(502, "Bad Gateway");
}
```

---

### **13. 503 Service Unavailable**
- **Meaning:** The server is temporarily unavailable.
- **Use Case:** A system under maintenance.

```csharp
private static bool _isMaintenanceMode = true;

[HttpGet("service-status")]
public IActionResult CheckStatus()
{
    if (_isMaintenanceMode) return StatusCode(503, "Service Unavailable");
    return Ok("Service Running");
}
```

---

### **14. 504 Gateway Timeout**
- **Meaning:** The server didn't receive a timely response.
- **Use Case:** A slow API request.

```csharp
[HttpGet("slow-service")]
public async Task<IActionResult> SlowService()
{
    await Task.Delay(7000); // Simulating delay (7 seconds)
    return Ok("Task completed");
}
```
---

### **Global Exception Handling for 5xx Errors**
Instead of handling errors in every controller, use **middleware**:

```csharp
public class GlobalExceptionMiddleware
{
    private readonly RequestDelegate _next;
    private readonly ILogger<GlobalExceptionMiddleware> _logger;

    public GlobalExceptionMiddleware(RequestDelegate next, ILogger<GlobalExceptionMiddleware> logger)
    {
        _next = next;
        _logger = logger;
    }

    public async Task Invoke(HttpContext context)
    {
        try
        {
            await _next(context);
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Unhandled exception occurred.");
            context.Response.StatusCode = 500;
            await context.Response.WriteAsync("Internal Server Error");
        }
    }
}
```
📌 **Usage:** Add middleware in `Program.cs`:
```csharp
app.UseMiddleware<GlobalExceptionMiddleware>();
```
Now, all unhandled exceptions return a **500 Internal Server Error** automatically.

---

## **Conclusion**
| **Status Code** | **Meaning** | **Example Scenario** |
|---------------|------------|----------------|
| **100** Continue | Server acknowledges request | Large file upload |
| **200** OK | Request successful | Fetching user details |
| **201** Created | New resource created | User registration |
| **301** Moved Permanently | Permanent redirect | Old URL to a new site |
| **302** Found | Temporary redirect | Maintenance mode |
| **400** Bad Request | Invalid request | Missing email in form submission |
| **401** Unauthorized | Authentication needed | Accessing profile without login |
| **403** Forbidden | Access denied | User without admin role accessing admin panel |
| **404** Not Found | Resource not found | Invalid API endpoint |
| **500** Internal Server Error | Unexpected failure | Database crash |
| **501** Not Implemented | Feature not available | Unfinished API method |
| **502** Bad Gateway | Upstream server failure | API Gateway failure |
| **503** Service Unavailable | Temporary downtime | Scheduled maintenance |
| **504** Gateway Timeout | Request timeout | NGINX timeout due to slow backend |

---