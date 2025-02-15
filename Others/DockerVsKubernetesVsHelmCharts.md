# Docker vs Kubernetes - Interview Questions

## Basic Questions

### 1. What is Docker?
Docker is a containerization platform that allows developers to package applications and their dependencies into lightweight, portable containers that run consistently across different environments.

### 2. What is Kubernetes?
Kubernetes is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications across clusters of machines.

### 3. How is Docker different from Kubernetes?

| Feature     | Docker | Kubernetes |
|------------|--------|------------|
| Purpose    | Containerization tool | Container orchestration platform |
| Deployment | Runs single containers | Manages multiple containers across clusters |
| Scaling    | Manual or Docker Swarm | Automatic scaling with HPA/VPA |
| Networking | Uses Docker bridge network | Uses CNI plugins like Calico, Flannel |

### 4. Can Docker run without Kubernetes?
Yes, Docker can run standalone without Kubernetes. However, Kubernetes requires a container runtime like Docker or containerd to run containers.

### 5. Is Kubernetes a replacement for Docker?
No, Kubernetes is not a replacement for Docker. Kubernetes is used for managing and orchestrating containerized applications, whereas Docker is a container runtime. Kubernetes can work with Docker but can also use other container runtimes like containerd or CRI-O.

---

## Intermediate Questions

### 6. What are the main components of Docker?
- **Docker Engine**: Runs and manages containers.
- **Docker CLI**: Command-line interface to interact with Docker.
- **Docker Compose**: Tool for defining and running multi-container Docker applications.
- **Docker Registry**: Stores container images (e.g., Docker Hub, AWS ECR).

### 7. What are the main components of Kubernetes?
- **Master Node**: Includes API server, scheduler, controller manager, and etcd.
- **Worker Nodes**: Run containerized applications using kubelet, kube-proxy, and container runtime.
- **Pods**: The smallest deployable unit in Kubernetes that encapsulates containers.
- **Deployments, Services, ConfigMaps, Secrets, etc.**: Used for application management.

### 8. How does Kubernetes manage container networking compared to Docker?
- **Docker** uses a simple bridge network by default.
- **Kubernetes** provides a flat network model using CNI (Container Network Interface) plugins like Calico, Flannel, or Cilium, allowing pod-to-pod communication across nodes.

### 9. What is the difference between Docker Compose and Kubernetes?
- **Docker Compose**: Used to define and run multi-container applications on a single host.
- **Kubernetes**: Designed for orchestrating containers across multiple hosts in a cluster.

### 10. What are Namespaces in Kubernetes, and does Docker have an equivalent?
- **Namespaces in Kubernetes** help isolate resources within a cluster.
- **Docker** has an equivalent concept using network namespaces, but it is not as advanced as Kubernetes namespaces.

---

## Advanced Questions

### 11. How do you deploy a Dockerized application in Kubernetes?
1. Push the Docker image to a registry (e.g., Docker Hub, AWS ECR).
2. Create Kubernetes manifests (Deployment, Service, Ingress).
3. Apply them using:
   ```sh
   kubectl apply -f <manifest>.yaml
   ```

### 12. How does Kubernetes handle scaling compared to Docker?
- In **Docker**, scaling needs to be handled manually using `docker-compose scale` or external tools.
- **Kubernetes** provides built-in **Horizontal Pod Autoscaler (HPA)** and **Vertical Pod Autoscaler (VPA)** to manage scaling automatically.

### 13. How do Kubernetes Pods communicate with each other?
- Kubernetes assigns a unique **IP** to each pod and allows communication via **Services** (ClusterIP, NodePort, LoadBalancer, etc.).
- Kubernetes uses **DNS resolution** to enable service discovery.

### 14. What is Helm, and how does it help with Kubernetes deployments?
Helm is a package manager for Kubernetes that helps deploy applications using reusable templates called **Helm charts**.

### 15. What are the alternatives to Docker for Kubernetes?
Kubernetes supports other container runtimes like:
- **containerd**
- **CRI-O**
- **Podman**

---

## Helm Chart Questions

### 16. What is a Helm Chart?
A Helm Chart is a collection of YAML files that define a set of Kubernetes resources, enabling easy application deployment and management.

### 17. What are the main components of a Helm Chart?
- **Chart.yaml**: Metadata about the chart.
- **values.yaml**: Default configuration values.
- **templates/**: Kubernetes manifest templates.
- **requirements.yaml**: List of dependencies (for Helm v2).
- **charts/**: Sub-charts or dependencies.

### 18. How do you install an application using Helm?
1. Add a Helm repository (if needed):
   ```sh
   helm repo add stable https://charts.helm.sh/stable
   ```
2. Install the chart:
   ```sh
   helm install my-app stable/nginx
   ```

### 19. How do you upgrade a Helm release?
   ```sh
   helm upgrade my-app stable/nginx
   ```

### 20. How do you rollback a Helm release?
   ```sh
   helm rollback my-app 1
   ```

### 21. How do you list all deployed Helm releases?
   ```sh
   helm list
   ```

### 22. How do you uninstall a Helm release?
   ```sh
   helm uninstall my-app
   ```

### 23. How can you override `values.yaml` when installing a Helm chart?
You can pass a custom values file using:
   ```sh
   helm install my-app -f custom-values.yaml stable/nginx
   ```

### 24. How do Helm and Kubernetes work together?
Helm simplifies Kubernetes application deployment by:
- Managing Kubernetes manifests.
- Handling templating and versioning.
- Allowing easy configuration overrides.

### 25. What is the difference between Helm v2 and Helm v3?

| Feature        | Helm v2 | Helm v3 |
|---------------|---------|---------|
| Tiller        | Required | Removed |
| Security      | Less secure | More secure (direct API access) |
| CRDs Handling | Manual | Automated |
| Charts        | `requirements.yaml` | `Chart.yaml` |

---

## Summary
- **Docker** is for containerization, while **Kubernetes** manages and orchestrates multiple containers.
- **Helm** simplifies Kubernetes deployments by using Helm charts.
- Kubernetes provides advanced networking, scaling, and auto-healing features compared to Docker alone.
- Helm allows for easier application management in Kubernetes using templated configurations.


