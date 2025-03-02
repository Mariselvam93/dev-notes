### **Azure Functions Overview**
Azure Functions is a **serverless compute service** provided by Microsoft Azure that allows developers to run event-driven code without managing infrastructure. It automatically scales based on demand and bills only for the execution time of the function.

### **Key Features**
1. **Event-Driven Execution** – Azure Functions can be triggered by various events like HTTP requests, database changes, message queues, or timers.
2. **Auto-Scaling** – Functions scale dynamically based on workload, making them ideal for applications with varying workloads.
3. **Multiple Language Support** – Supports C#, JavaScript, Python, PowerShell, Java, and more.
4. **Bindings** – Provides input/output bindings to connect with Azure services like Blob Storage, Cosmos DB, Service Bus, and more.
5. **Consumption-Based Pricing** – Charges only for the actual execution time and resources used.
6. **Security & Authentication** – Integrates with Azure AD, API Management, and Key Vault for secure execution.

---

### **Use Cases of Azure Functions**
1. **Serverless APIs & Microservices**
   - Build lightweight APIs that scale automatically.
   - Example: An API function that fetches data from Cosmos DB and returns a response.

2. **Data Processing & Transformation**
   - Process data from Azure Blob Storage, Service Bus, or Event Hubs.
   - Example: A function that resizes images after they are uploaded to a Blob Storage container.

3. **Scheduled Jobs (Cron Jobs)**
   - Automate tasks using time-based triggers.
   - Example: A function that runs every night to clean up logs in a database.

4. **Event-Driven Workflows**
   - Trigger functions based on events from other Azure services.
   - Example: A function that sends an email notification when a new user registers.

5. **IoT Data Processing**
   - Handle real-time data from IoT devices.
   - Example: A function that processes sensor data and stores it in Azure Table Storage.

6. **AI & Machine Learning Inference**
   - Execute AI/ML models in real-time without provisioning dedicated infrastructure.
   - Example: A function that analyzes sentiment from incoming text messages using Azure Cognitive Services.

7. **Real-Time Stream Processing**
   - Process streaming data from Azure Event Hubs.
   - Example: A function that monitors financial transactions for fraud detection.

8. **Automating CI/CD Pipelines**
   - Integrate with DevOps tools for automation.
   - Example: A function that triggers a deployment process in Azure DevOps when a new commit is pushed.

---

### **Azure Functions Hosting Plans**
1. **Consumption Plan** (Default)
   - Fully serverless, scales automatically.
   - Pay-per-use pricing model.

2. **Premium Plan**
   - Runs on pre-warmed instances for better performance.
   - Ideal for low-latency applications.

3. **Dedicated (App Service) Plan**
   - Runs on dedicated Azure App Service infrastructure.
   - Best for applications with consistent workloads.

---

### **Conclusion**
Azure Functions is a powerful serverless computing service ideal for event-driven applications, real-time data processing, and automation tasks. It simplifies development, reduces costs, and provides built-in scalability, making it a great choice for modern cloud-native applications.

---
Here’s a simple example of an **HTTP-triggered Azure Function** using **.NET 8 (C# minimal API style)**.

---

## **Step 1: Create an Azure Functions Project**
Run the following command in your terminal to create a new Azure Functions project:

```sh
func init MyFunctionApp --worker-runtime dotnet
cd MyFunctionApp
```

---

## **Step 2: Create a New Function**
Run this command to create an HTTP-triggered function:

```sh
func new --name MyHttpFunction --template "HTTP trigger" --authlevel "anonymous"
```

This creates a file `MyHttpFunction.cs` inside the `Functions` directory.

---

## **Step 3: Implement the Azure Function in C#**
Modify the `MyHttpFunction.cs` file as follows:

```csharp
using Microsoft.Azure.Functions.Worker;
using Microsoft.Azure.Functions.Worker.Http;
using Microsoft.Extensions.Logging;
using System.Net;

public class MyHttpFunction
{
    private readonly ILogger<MyHttpFunction> _logger;

    public MyHttpFunction(ILogger<MyHttpFunction> logger)
    {
        _logger = logger;
    }

    [Function("MyHttpFunction")]
    public HttpResponseData Run(
        [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")] HttpRequestData req)
    {
        _logger.LogInformation("C# HTTP trigger function processed a request.");

        var response = req.CreateResponse(HttpStatusCode.OK);
        response.Headers.Add("Content-Type", "text/plain; charset=utf-8");

        response.WriteString("Hello from Azure Functions!");

        return response;
    }
}
```

---

## **Step 4: Run the Function Locally**
Start the function app locally using:

```sh
func start
```

You should see output like:

```
Http Function:
   Get  | http://localhost:7071/api/MyHttpFunction
```

Open the URL in your browser or test with:

```sh
curl http://localhost:7071/api/MyHttpFunction
```

---

## **Step 5: Deploy to Azure**
To deploy the function to Azure, first log in:

```sh
az login
```

Then create a **Function App** in Azure and deploy:

```sh
func azure functionapp publish <YOUR_FUNCTION_APP_NAME>
```

---

### **Conclusion**
This function listens for HTTP requests and returns a simple response. You can extend it by integrating with Azure Storage, Cosmos DB, or Service Bus.

---
Here’s an **Azure Function with Azure Blob Storage integration** in **.NET 8 (Isolated Worker Model)**.

---

## **Scenario**
We will create an **HTTP-triggered** Azure Function that **uploads a file to Azure Blob Storage** when a user sends a file in the request.

---

## **Step 1: Prerequisites**
1. Install the **Azure Functions Core Tools**:  
   ```sh
   npm install -g azure-functions-core-tools@4 --unsafe-perm true
   ```

2. Install the **Azure Storage Emulator (Azurite)** (for local testing):  
   ```sh
   npm install -g azurite
   ```

3. Start the storage emulator:  
   ```sh
   azurite
   ```

---

## **Step 2: Create the Azure Functions Project**
Create a new **.NET 8 isolated worker** Azure Functions project:

```sh
func init BlobFunctionApp --worker-runtime dotnet-isolated
cd BlobFunctionApp
```

---

## **Step 3: Create a New HTTP-Triggered Function**
Run the following command to generate a new HTTP function:

```sh
func new --name UploadFileFunction --template "HTTP trigger" --authlevel "anonymous"
```

---

## **Step 4: Install Required NuGet Packages**
In the project directory, install the required Azure SDKs:

```sh
dotnet add package Azure.Storage.Blobs
dotnet add package Microsoft.Azure.Functions.Worker.Extensions.Storage
```

---

## **Step 5: Implement the Function to Upload Files to Blob Storage**
Modify `UploadFileFunction.cs`:

```csharp
using System.IO;
using System.Net;
using System.Threading.Tasks;
using Azure.Storage.Blobs;
using Microsoft.Azure.Functions.Worker;
using Microsoft.Azure.Functions.Worker.Http;
using Microsoft.Extensions.Logging;

public class UploadFileFunction
{
    private readonly ILogger<UploadFileFunction> _logger;
    private const string ConnectionString = "UseDevelopmentStorage=true"; // Local Emulator

    public UploadFileFunction(ILogger<UploadFileFunction> logger)
    {
        _logger = logger;
    }

    [Function("UploadFileFunction")]
    public async Task<HttpResponseData> Run(
        [HttpTrigger(AuthorizationLevel.Anonymous, "post")] HttpRequestData req)
    {
        _logger.LogInformation("File upload function triggered.");

        // Check if file is provided
        if (!req.Headers.TryGetValues("Content-Type", out var contentType) || 
            !contentType.Contains("multipart/form-data"))
        {
            var badRequest = req.CreateResponse(HttpStatusCode.BadRequest);
            await badRequest.WriteStringAsync("Invalid file format. Please upload a valid file.");
            return badRequest;
        }

        // Read the file from request
        var stream = req.Body;
        string blobName = $"upload-{System.Guid.NewGuid()}.txt"; // Change extension as needed

        // Upload to Azure Blob Storage
        var blobServiceClient = new BlobServiceClient(ConnectionString);
        var blobContainerClient = blobServiceClient.GetBlobContainerClient("uploads");
        await blobContainerClient.CreateIfNotExistsAsync();

        var blobClient = blobContainerClient.GetBlobClient(blobName);
        await blobClient.UploadAsync(stream, overwrite: true);

        // Return response
        var response = req.CreateResponse(HttpStatusCode.OK);
        await response.WriteStringAsync($"File uploaded successfully: {blobClient.Uri}");

        return response;
    }
}
```

---

## **Step 6: Configure Storage Settings**
In `local.settings.json`, update the storage connection string:

```json
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "UseDevelopmentStorage=true",
    "FUNCTIONS_WORKER_RUNTIME": "dotnet-isolated"
  }
}
```

---

## **Step 7: Run the Function Locally**
Start the function locally:

```sh
func start
```

It should output:

```
Http Function:
   Post | http://localhost:7071/api/UploadFileFunction
```

---

## **Step 8: Test the Function**
Use **cURL** to test the function:

```sh
curl -X POST "http://localhost:7071/api/UploadFileFunction" \
     -H "Content-Type: multipart/form-data" \
     -F "file=@sample.txt"
```

Or use **Postman**:
- Set **Method**: `POST`
- **URL**: `http://localhost:7071/api/UploadFileFunction`
- **Body**: Select `form-data` → `file` → Choose a file

You should see a response:

```
File uploaded successfully: http://127.0.0.1:10000/devstoreaccount1/uploads/upload-xxxxxxxx.txt
```

---

## **Step 9: Deploy to Azure**
First, **create a storage account** in Azure:

```sh
az storage account create --name mystorageaccount --resource-group myResourceGroup --location eastus --sku Standard_LRS
```

Then, deploy the function:

```sh
func azure functionapp publish <YOUR_FUNCTION_APP_NAME>
```

---

## **Conclusion**
This Azure Function:
✅ Accepts file uploads via HTTP  
✅ Stores files in **Azure Blob Storage**  
✅ Uses the **Azure SDK for Blob Storage**  
✅ Works both **locally (Azurite)** and in **Azure**  

