# **Kubernetes: The Big Picture**  

Kubernetes (K8s) is an open-source container orchestration platform designed to manage, deploy, and scale containerized applications efficiently. In a distributed microservices architecture, such as an e-commerce platform built with .NET, Kubernetes provides automated scaling, self-healing, service discovery, and declarative configuration.

---

## **1️⃣ Kubernetes Architecture & Core Components**  
Kubernetes consists of a **Control Plane** and **Worker Nodes** that manage the lifecycle of applications.

### **1.1 Control Plane Components**  
The control plane manages cluster state and scheduling. It consists of:

- **API Server (`kube-apiserver`)**  
  - Central control component that exposes Kubernetes APIs.
  - All interactions (kubectl, clients, controllers) go through this.
  
- **Controller Manager (`kube-controller-manager`)**  
  - Manages controllers that ensure the desired state of Kubernetes objects, including:
    - **Deployment Controller** (ensures Pods are up and running)
    - **Node Controller** (monitors and handles node failures)
    - **Job Controller** (runs batch jobs)
  
- **Scheduler (`kube-scheduler`)**  
  - Assigns pods to worker nodes based on available resources, affinity, and constraints.

- **ETCD (Database Storage)**  
  - Stores the entire cluster state.
  - Highly available, distributed key-value store.

- **Cloud Controller Manager** *(optional, for cloud integrations)*  
  - Manages interactions with cloud providers (AWS, Azure, GCP).
  - Handles LoadBalancers, storage provisioning, and networking.

---

### **1.2 Worker Node Components**  
Worker nodes run application workloads.

- **Kubelet**  
  - Agent running on each node that ensures containers are running.

- **Container Runtime**  
  - Runs containers (Docker, containerd, CRI-O).

- **Kube Proxy**  
  - Maintains network rules and enables service communication.

- **Pods**  
  - The smallest unit in Kubernetes, containing one or more containers.
  
---

## **2️⃣ Essential Kubernetes Objects**
Objects in Kubernetes represent the desired state of the system.

### **2.1 Workloads**
- **Pod**: Smallest deployable unit; contains container(s).
- **Deployment**: Manages pod replicas, rolling updates, and rollbacks.
- **StatefulSet**: Manages stateful applications like databases (SQL Server, MongoDB).
- **DaemonSet**: Runs a pod on every node (e.g., logging agents, monitoring).
- **Job / CronJob**: Runs batch tasks (e.g., order processing).

### **2.2 Networking**
- **Service**: Exposes applications within or outside the cluster.
  - **ClusterIP** (internal service)
  - **NodePort** (exposes service on each node's IP)
  - **LoadBalancer** (uses cloud provider’s load balancer)
  - **Ingress** (exposes multiple services via domain paths)
- **Ingress Controller**: Routes external traffic to services (e.g., NGINX Ingress).

### **2.3 Storage**
- **PersistentVolume (PV)** & **PersistentVolumeClaim (PVC)**: Manage storage.
- **ConfigMap** & **Secrets**: Store environment variables, credentials.

### **2.4 Security & Policies**
- **ServiceAccount**: Defines permissions for pods.
- **NetworkPolicy**: Controls traffic between pods.

---

## **3️⃣ Essential Files in Kubernetes**
These files are typically written in YAML (`.yaml`) format.

### **3.1 Deployment File**
Defines how a microservice should be deployed.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: product-service
spec:
  replicas: 3
  selector:
    matchLabels:
      app: product-service
  template:
    metadata:
      labels:
        app: product-service
    spec:
      containers:
      - name: product-service
        image: myrepo/product-service:latest
        ports:
        - containerPort: 80
```

### **3.2 Service File**
Defines how the microservice can be accessed.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: product-service
spec:
  type: ClusterIP
  selector:
    app: product-service
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```

### **3.3 Ingress File**
Routes external traffic via domain-based routing.

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ecom-ingress
spec:
  rules:
  - host: shop.example.com
    http:
      paths:
      - path: /products
        pathType: Prefix
        backend:
          service:
            name: product-service
            port:
              number: 80
```

---

## **4️⃣ Step-by-Step Breakdown for E-commerce Microservices**
### **4.1 E-commerce Architecture Overview**
In a typical e-commerce platform with microservices using .NET, we have:

- **Product Service** (Manages product catalog)
- **Order Service** (Handles orders, payment, and checkout)
- **User Service** (Manages user authentication, profile, etc.)
- **Inventory Service** (Manages stock availability)
- **Payment Service** (Processes payments)
- **API Gateway** (NGINX or .NET API Gateway to route requests)
- **Database (SQL Server / PostgreSQL / MongoDB)** (Stores data)

### **4.2 Deploying E-commerce Services in Kubernetes**
#### **1️⃣ Install Kubernetes (if not installed)**
For local development, install **Minikube** or use **Docker Desktop**.

```sh
minikube start
```

For production, use **AWS EKS / Azure AKS / GCP GKE**.

#### **2️⃣ Create Deployment & Service for Product Service**
```sh
kubectl apply -f product-deployment.yaml
kubectl apply -f product-service.yaml
```

#### **3️⃣ Verify Deployment**
```sh
kubectl get pods
kubectl get svc
```

#### **4️⃣ Set Up Ingress for Domain Routing**
```sh
kubectl apply -f ingress.yaml
```

#### **5️⃣ Check Ingress Configuration**
```sh
kubectl get ingress
```

---

## **5️⃣ Monitoring & Scaling**
### **5.1 Autoscaling (Horizontal Pod Autoscaler)**
Automatically scales pods based on CPU/memory usage.

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: product-service-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: product-service
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50
```

Apply it:
```sh
kubectl apply -f hpa.yaml
```

### **5.2 Logging & Monitoring**
- Use **Prometheus** & **Grafana** for monitoring.
- Use **Fluentd** or **ELK Stack** for logging.

---

## **6️⃣ Summary**
| Component | Description |
|-----------|-------------|
| **API Server** | Manages cluster communication |
| **Controller Manager** | Ensures desired state |
| **Scheduler** | Assigns pods to nodes |
| **ETCD** | Stores cluster state |
| **Kubelet** | Runs on worker nodes |
| **Pod** | Smallest deployable unit |
| **Service** | Exposes application internally or externally |
| **Ingress** | Handles domain-based routing |
| **PVC/PV** | Manages persistent storage |
| **ConfigMap/Secret** | Stores configuration and credentials |

---

## **7️⃣ Conclusion**
By leveraging Kubernetes, an e-commerce microservices platform built in .NET can achieve:
✔ **Scalability** – Auto-scale based on demand.  
✔ **High Availability** – Self-healing & failover.  
✔ **Service Discovery** – Seamless communication.  
✔ **Security** – Network policies & secrets management.  
✔ **Storage Management** – Persistent storage integration.

## **Step-by-Step Explanation of Kubernetes Deployment YAML File**  

A **Deployment** in Kubernetes manages a set of **replica pods** and ensures that the desired number of **healthy** pods are running at all times. It supports **rolling updates, rollbacks, and scaling**.

---

### **Step 1: Define the API Version**
```yaml
apiVersion: apps/v1
```
- Specifies the API version being used (`apps/v1` is the latest stable version for Deployments).
- Ensures compatibility with the Kubernetes API.

---

### **Step 2: Specify the Kind**
```yaml
kind: Deployment
```
- Defines the type of Kubernetes object (in this case, a **Deployment**).
- Other kinds include **Pod, Service, StatefulSet, ConfigMap, etc.**

---

### **Step 3: Metadata (Name and Labels)**
```yaml
metadata:
  name: product-service
```
- Specifies the **name** of the Deployment (`product-service`).
- Used for identifying and managing this Deployment.

---

### **Step 4: Define the Desired Number of Replicas**
```yaml
spec:
  replicas: 3
```
- Defines the number of pod instances (**replicas**) that Kubernetes should maintain.
- Ensures **high availability** and **load distribution** across worker nodes.
- Kubernetes will create **3 pods** for this service.

---

### **Step 5: Selector to Match Labels**
```yaml
  selector:
    matchLabels:
      app: product-service
```
- Selects the pods that **belong to this deployment**.
- Ensures the deployment only manages pods labeled `app: product-service`.

---

### **Step 6: Pod Template**
```yaml
  template:
    metadata:
      labels:
        app: product-service
```
- Defines the pod **blueprint** that Kubernetes will use to create each replica.
- The `labels` ensure the selector can correctly match these pods.

---

### **Step 7: Pod Specification**
```yaml
    spec:
      containers:
```
- Defines the container(s) that will run inside each pod.

---

### **Step 8: Container Details**
```yaml
      - name: product-service
        image: myrepo/product-service:latest
```
- **name**: Name of the container (`product-service`).
- **image**: The Docker image to pull from a registry (`myrepo/product-service:latest`).
  - If using Docker Hub: `myrepo/product-service:latest`
  - If using AWS ECR: `123456789012.dkr.ecr.us-east-1.amazonaws.com/product-service:latest`
  - If using private registry, authentication may be needed.

---

### **Step 9: Exposing Container Ports**
```yaml
        ports:
        - containerPort: 80
```
- **containerPort**: The port on which the container will listen inside the pod.
- This does **not** expose the service externally (that requires a Service YAML).

---

### **Summary of the Deployment File**
| **Field**            | **Description** |
|----------------------|----------------|
| `apiVersion: apps/v1` | Defines the API version for the Deployment |
| `kind: Deployment` | Specifies that this is a Deployment object |
| `metadata.name` | Names the Deployment (`product-service`) |
| `spec.replicas: 3` | Defines 3 replicas (pods) |
| `selector.matchLabels` | Ensures the Deployment manages only pods labeled `app: product-service` |
| `template.metadata.labels` | Labels new pods with `app: product-service` |
| `spec.containers.name` | Names the container inside the pod (`product-service`) |
| `spec.containers.image` | Specifies the Docker image (`myrepo/product-service:latest`) |
| `spec.containers.ports.containerPort: 80` | Exposes port `80` inside the pod |

---

## **Other Important Kubernetes YAML Files**
To fully deploy and access the `product-service`, we need additional files.

### **1️⃣ Service YAML (product-service.yaml)**
A **Service** exposes the pods created by the Deployment.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: product-service
spec:
  type: ClusterIP
  selector:
    app: product-service
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
```

#### **Breakdown**
- **`kind: Service`** → Defines a Service resource.
- **`type: ClusterIP`** → Exposes the service internally within the cluster.
- **`selector`** → Targets pods with `app: product-service`.
- **`ports`** → Maps **port 80 of the service** to **port 80 of the container**.

> To expose the service externally, change `type: ClusterIP` to `type: LoadBalancer` or `type: NodePort`.

---

### **2️⃣ Ingress YAML (ingress.yaml)**
An **Ingress** routes external traffic to the correct service.

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: product-ingress
spec:
  rules:
  - host: shop.example.com
    http:
      paths:
      - path: /products
        pathType: Prefix
        backend:
          service:
            name: product-service
            port:
              number: 80
```

#### **Breakdown**
- **`kind: Ingress`** → Defines an Ingress resource.
- **`rules`** → Specifies routing rules.
  - Requests to **`shop.example.com/products`** go to the **`product-service`**.
- **`backend`** → Maps requests to `product-service:80`.

---

### **3️⃣ ConfigMap YAML (configmap.yaml)**
A **ConfigMap** stores environment variables.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: product-config
data:
  DATABASE_URL: "postgres://db-user:password@database:5432/products"
  LOG_LEVEL: "info"
```

#### **Breakdown**
- **Stores configuration variables** like database URLs.
- Avoids hardcoding values inside the application.
- Can be mounted as environment variables inside pods.

---

### **4️⃣ Secret YAML (secret.yaml)**
A **Secret** stores sensitive information securely.

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: product-secret
type: Opaque
data:
  DB_PASSWORD: cGFzc3dvcmQ=  # base64 encoded "password"
```

#### **Breakdown**
- Used to **store credentials securely**.
- `DB_PASSWORD` is **base64 encoded** (`echo -n "password" | base64`).
- Can be referenced in **Deployments** as environment variables.

---

### **5️⃣ Horizontal Pod Autoscaler YAML (hpa.yaml)**
Enables **autoscaling** based on CPU usage.

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: product-service-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: product-service
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50
```

#### **Breakdown**
- **`minReplicas: 2`** → Minimum 2 pods running.
- **`maxReplicas: 10`** → Scale up to 10 pods if needed.
- **CPU-based autoscaling** → If CPU usage exceeds 50%, scale up.

---

## **Deployment Process**
### **1️⃣ Apply ConfigMap & Secret**
```sh
kubectl apply -f configmap.yaml
kubectl apply -f secret.yaml
```

### **2️⃣ Deploy the Product Service**
```sh
kubectl apply -f product-deployment.yaml
```

### **3️⃣ Create the Service**
```sh
kubectl apply -f product-service.yaml
```

### **4️⃣ Set Up Ingress**
```sh
kubectl apply -f ingress.yaml
```

### **5️⃣ Enable Autoscaling**
```sh
kubectl apply -f hpa.yaml
```

---

## **Conclusion**
- The **Deployment YAML** defines **how** the pods are created and managed.
- A **Service** exposes them within or outside the cluster.
- **Ingress** routes traffic to services based on domain rules.
- **ConfigMap & Secrets** store non-sensitive and sensitive configurations.
- **HPA** automatically scales the service based on CPU usage.

