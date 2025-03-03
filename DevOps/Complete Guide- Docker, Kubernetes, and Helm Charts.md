

# **🚀 Docker, Kubernetes, and Helm Charts - A Complete Guide**  

## **🔹 1. Docker - Containerization Platform**  
### **📌 What is Docker?**  
Docker is a **containerization tool** that allows applications to be packaged with all dependencies and run consistently across different environments.  

### **📌 Why Use Docker?**  
✅ Ensures **consistent environments** in development, testing, and production.  
✅ **Lightweight** compared to Virtual Machines (VMs).  
✅ **Portable** - Run anywhere (local, cloud, or on-premise).  
✅ **Fast deployment** with minimal resource consumption.  

---

### **📌 Key Docker Files & Explanation**  
👉 **1. Dockerfile** (Defines the container image)  
👉 **2. .dockerignore** (Files to exclude when building the image)  
👉 **3. docker-compose.yml** (For managing multiple containers)  

---

### **📌 Step-by-Step: Creating a Dockerfile**  
A **Dockerfile** is a script containing instructions to **build** an image.  
📄 **Example: `Dockerfile` for a .NET 8 Web API**  

```dockerfile
# 1️⃣ Use the official .NET 8 SDK image as the base image
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build

# 2️⃣ Set the working directory inside the container
WORKDIR /app

# 3️⃣ Copy everything from the local project directory to the container
COPY . .

# 4️⃣ Restore dependencies
RUN dotnet restore

# 5️⃣ Build the application in Release mode
RUN dotnet build --configuration Release --output /app/build

# 6️⃣ Publish the application for production use
RUN dotnet publish --configuration Release --output /app/publish

# 7️⃣ Use the lightweight .NET runtime image for final container
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS runtime

# 8️⃣ Set the working directory inside the container
WORKDIR /app

# 9️⃣ Copy the published app from the build container
COPY --from=build /app/publish .

# 🔟 Expose the application's port
EXPOSE 8080

# 1️⃣1️⃣ Define the entry point command
CMD ["dotnet", "MyApp.dll"]
```

### **📌 Explanation of Dockerfile Steps**  
1️⃣ **FROM** – Uses the official .NET 8 SDK image to build the application.  
2️⃣ **WORKDIR** – Sets the working directory inside the container.  
3️⃣ **COPY** – Copies all files from the local project directory.  
4️⃣ **RUN dotnet restore** – Installs required dependencies.  
5️⃣ **RUN dotnet build** – Builds the .NET app in Release mode.  
6️⃣ **RUN dotnet publish** – Publishes the application for production.  
7️⃣ **FROM runtime** – Uses a lightweight runtime image for better performance.  
8️⃣ **WORKDIR /app** – Moves to the final working directory.  
9️⃣ **COPY --from=build** – Copies only the published app to reduce container size.  
🔟 **EXPOSE 8080** – Opens port 8080 for external requests.  
1️⃣1️⃣ **CMD ["dotnet", "MyApp.dll"]** – Runs the app inside the container.  

---

### **📌 Running Docker Commands**
```sh
# 1. Build the Docker image
docker build -t myapp .

# 2. Run the Docker container
docker run -d -p 8080:80 myapp

# 3. Push the image to Azure Container Registry
docker tag myapp myregistry.azurecr.io/myapp:v1
docker push myregistry.azurecr.io/myapp:v1
```

---

## **🔹 2. Kubernetes - Container Orchestration Platform**  
### **📌 What is Kubernetes?**  
Kubernetes (K8s) is a **container orchestration tool** that automates:  
✅ **Deployment** – Running multiple instances of a containerized app.  
✅ **Scaling** – Automatically increasing or decreasing the number of replicas.  
✅ **Load Balancing** – Distributing traffic evenly.  
✅ **Self-Healing** – Restarting failed containers automatically.  

---

### **📌 Key Kubernetes Files & Explanation**
👉 **1. Deployment YAML** – Defines how the application is deployed.  
👉 **2. Service YAML** – Exposes the application to the network.  
👉 **3. Ingress YAML** – Handles domain-based routing (Optional).  

---

### **📌 Step-by-Step: Kubernetes Deployment**  
📄 **Example: `deployment.yaml` (Deploying a .NET 8 API in AKS)**  
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp
        image: myregistry.azurecr.io/myapp:v1
        ports:
        - containerPort: 80
```

📌 **Explanation of Deployment.yaml**  
- **replicas: 3** – Runs 3 instances of the application.  
- **image: myregistry.azurecr.io/myapp:v1** – Pulls the image from the Azure Container Registry.  
- **containerPort: 80** – The app runs on port 80 inside the container.  

📄 **Example: `service.yaml` (Exposing the app in Kubernetes)**  
```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  type: LoadBalancer
  selector:
    app: myapp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```

📌 **Explanation of Service.yaml**  
- **type: LoadBalancer** – Exposes the app to external traffic.  
- **targetPort: 80** – Maps the container's port to the service port.  

📄 **Example: `ingress.yaml` (Using Ingress for Custom Domains)**  
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: myapp-ingress
spec:
  rules:
  - host: myapp.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: myapp-service
            port:
              number: 80
```

📌 **Explanation of Ingress.yaml**  
- **host: myapp.example.com** – Maps a custom domain to the service.  
- **backend: myapp-service** – Routes traffic to the `myapp-service`.  

### **📌 Applying Kubernetes Files**
```sh
# Deploy to AKS
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl apply -f ingress.yaml
```

---

## **🔹 3. Helm Charts - Kubernetes Package Manager**  
### **📌 What is Helm?**  
Helm is a **package manager** for Kubernetes that automates deployment using **templated configurations**.

📄 **Example: `values.yaml` (Configurable Parameters for Helm Chart)**  
```yaml
replicaCount: 2
image:
  repository: myregistry.azurecr.io/myapp
  tag: v1
service:
  type: LoadBalancer
  port: 80
```

### **📌 Deploying with Helm**
```sh
# Create a Helm chart
helm create mychart

# Install the Helm chart
helm install myapp ./mychart

# Upgrade or rollback
helm upgrade myapp ./mychart
helm rollback myapp 1
```

---

## **🔥 Summary**
| **Tool** | **Purpose** |
|----------|------------|
| **Docker** 🐳 | Packages apps into containers, Builds & runs containers |
| **Kubernetes** ☸️ | Orchestrates and manages containers |
| **Helm** 🎢 | Automates Kubernetes deployments |

---


## **🚀 E-Commerce Microservices Example**

### **📌 Section 1: Dockerizing a .NET 8 Application**  
📄 **Dockerfile** – Defines the container image  

```dockerfile
# Base image with .NET SDK for building
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /app
COPY . .
RUN dotnet restore
RUN dotnet build --configuration Release --output /app/build
RUN dotnet publish --configuration Release --output /app/publish

# Final lightweight runtime image
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS runtime
WORKDIR /app
COPY --from=build /app/publish .
EXPOSE 8080
CMD ["dotnet", "MyApp.dll"]
```

**Build & Run the Image**  
```sh
docker build -t myapp .
docker run -d -p 8080:8080 myapp
docker tag myapp myregistry.azurecr.io/myapp:v1
docker push myregistry.azurecr.io/myapp:v1
```

---

### **📌 Section 2: Kubernetes Deployment Files**  

📄 **`deployment.yaml`** – Defines how the app is deployed in AKS  

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp
        image: myregistry.azurecr.io/myapp:v1
        ports:
        - containerPort: 80
```

📄 **`service.yaml`** – Exposes the app  

```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  type: LoadBalancer
  selector:
    app: myapp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```

📄 **`ingress.yaml`** – Defines domain routing  

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: myapp-ingress
spec:
  rules:
  - host: myapp.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: myapp-service
            port:
              number: 80
```

**Deploy to AKS**  
```sh
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl apply -f ingress.yaml
```

---

### **📌 Section 3: Helm Chart for Deployment**  

📄 **Helm `values.yaml`**  

```yaml
replicaCount: 2
image:
  repository: myregistry.azurecr.io/myapp
  tag: v1
service:
  type: LoadBalancer
  port: 80
```

**Deploy with Helm**  
```sh
helm install myapp ./mychart
```

---

### **📌 Section 4: Azure DevOps CI/CD Pipeline for AKS**  

📄 **Azure DevOps YAML (`azure-pipelines.yml`)**  

```yaml
trigger:
- main

pool:
  vmImage: 'ubuntu-latest'

variables:
  acrName: 'myregistry'
  aksCluster: 'my-aks-cluster'
  imageName: 'myapp'
  tag: 'v1'

stages:
- stage: Build
  jobs:
  - job: Build
    steps:
    - task: DotNetCoreCLI@2
      inputs:
        command: 'build'
        projects: '**/*.csproj'
    
    - task: Docker@2
      inputs:
        command: 'buildAndPush'
        repository: '$(acrName).azurecr.io/$(imageName)'
        dockerfile: '**/Dockerfile'
        tags: '$(tag)'

- stage: Deploy
  jobs:
  - job: DeployToAKS
    steps:
    - task: Kubernetes@1
      inputs:
        connectionType: 'Azure Resource Manager'
        azureSubscription: 'MyAzureSubscription'
        azureResourceGroup: 'myResourceGroup'
        kubernetesCluster: '$(aksCluster)'
        namespace: 'default'
        command: 'apply'
        arguments: '-f k8s/deployment.yaml'
```

---

### **📌 Section 5: E-Commerce Microservices Architecture with .NET 8**  

#### **1️⃣ User Service (`User.API`)**  
- **Registers users**  
- **Handles authentication**  

```csharp
[ApiController]
[Route("api/users")]
public class UserController : ControllerBase
{
    private readonly IUserService _userService;
    
    public UserController(IUserService userService)
    {
        _userService = userService;
    }

    [HttpPost("register")]
    public async Task<IActionResult> Register(UserDto user)
    {
        var result = await _userService.RegisterAsync(user);
        return Ok(result);
    }
}
```

---

#### **2️⃣ Product Service (`Product.API`)**  
- **Manages products and inventory**  

```csharp
[ApiController]
[Route("api/products")]
public class ProductController : ControllerBase
{
    private readonly IProductService _productService;
    
    public ProductController(IProductService productService)
    {
        _productService = productService;
    }

    [HttpGet]
    public async Task<IActionResult> GetAllProducts()
    {
        var products = await _productService.GetAllAsync();
        return Ok(products);
    }
}
```

---

#### **3️⃣ Order Service (`Order.API`)**  
- **Processes orders**  

```csharp
[ApiController]
[Route("api/orders")]
public class OrderController : ControllerBase
{
    private readonly IOrderService _orderService;
    
    public OrderController(IOrderService orderService)
    {
        _orderService = orderService;
    }

    [HttpPost]
    public async Task<IActionResult> CreateOrder(OrderDto order)
    {
        var result = await _orderService.CreateAsync(order);
        return Ok(result);
    }
}
```

---

### **📌 Section 6: Deploying Microservices to AKS using Helm**  

📄 **`helm/values.yaml`**  

```yaml
services:
  user:
    image: myregistry.azurecr.io/user-api:v1
    replicas: 2
  product:
    image: myregistry.azurecr.io/product-api:v1
    replicas: 2
  order:
    image: myregistry.azurecr.io/order-api:v1
    replicas: 2
```

**Deploy with Helm**  
```sh
helm install ecommerce ./helm
```

---

## **🚀 Conclusion**
- **Docker**: Containerized our services.  
- **Kubernetes (AKS)**: Managed our deployments.  
- **Helm**: Automated Kubernetes deployments.  
- **Azure DevOps**: CI/CD pipeline for automated builds and deployments.  
- **Microservices**: Implemented an **e-commerce** architecture with **User, Product, and Order services**.

---


Docker, Kubernetes, and Helm Charts are related but serve different purposes in containerized application deployment. Here’s a breakdown of their differences:

### 1. **Docker** 🐳  
   - **What it is**: A platform for **building, packaging, and running containers**.  
   - **Purpose**: It allows you to create lightweight, portable containers that can run applications along with their dependencies in an isolated environment.  
   - **Key Features**:  
     - Image-based containerization  
     - Portability across environments  
     - Efficient resource utilization  
     - Supports Docker Compose for multi-container applications  

   **Example Usage**:  
   - Building an image:  
     ```sh
     docker build -t myapp .
     ```
   - Running a container:  
     ```sh
     docker run -d -p 8080:80 myapp
     ```

---

### 2. **Kubernetes (K8s)** ☸️  
   - **What it is**: An **orchestration platform** for managing and scaling containerized applications across multiple nodes.  
   - **Purpose**: Automates deployment, scaling, and management of containerized workloads.  
   - **Key Features**:  
     - Automatic scaling of applications  
     - Load balancing and service discovery  
     - Self-healing (restarts failed containers)  
     - Declarative configuration using YAML  

   **Example Usage**:  
   - Deploy an application to Kubernetes:  
     ```sh
     kubectl apply -f deployment.yaml
     ```
   - Get running pods:  
     ```sh
     kubectl get pods
     ```

---

### 3. **Helm Charts** 🎢  
   - **What it is**: A **package manager for Kubernetes** that simplifies deployment and management of applications.  
   - **Purpose**: Helps manage Kubernetes manifests using reusable and configurable templates.  
   - **Key Features**:  
     - Simplifies complex Kubernetes deployments  
     - Supports versioning and rollbacks  
     - Uses **charts** (predefined Kubernetes YAML templates)  
     - Allows parameterization for customization  

   **Example Usage**:  
   - Install a Helm chart:  
     ```sh
     helm install myrelease mychart/
     ```
   - Upgrade a Helm release:  
     ```sh
     helm upgrade myrelease mychart/
     ```

---

### 🔥 **Key Differences**
| Feature      | Docker 🐳 | Kubernetes ☸️ | Helm Charts 🎢 |
|-------------|----------|-------------|--------------|
| **Purpose** | Runs containers | Manages containers | Manages K8s deployments |
| **Scope** | Single container | Cluster of containers | Templates for K8s apps |
| **Management** | Manual | Automated | Config-driven |
| **Scaling** | Not built-in | Auto-scaling | Simplifies scaling via configs | 
| **Ease of Use** | Simple | Complex | Easier for K8s |

---

### 🚀 **How They Work Together**
1. **Docker**: Packages the app into a container.
2. **Kubernetes**: Deploys and manages these containers across multiple nodes.
3. **Helm**: Simplifies Kubernetes deployments by managing YAML configurations.

