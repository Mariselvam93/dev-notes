# **CI/CD Flow Overview**
### **1. Code Commit & Version Control**  
   - Developers push code to a Git repository (Azure Repos, GitHub, GitLab, Bitbucket).
   - CI/CD pipeline gets triggered.

### **2. Build & Test (.NET 8 API)**
   - Restore NuGet packages.
   - Run security scans (Snyk, SonarQube).
   - Build the application.
   - Run unit and integration tests.

### **3. Containerization (Docker)**
   - Build a Docker image for the API.
   - Push the image to a container registry (Azure Container Registry, AWS ECR, Docker Hub).

### **4. Deployment to Kubernetes (AKS, EKS, On-Prem)**
   - Use Helm charts for deployment.
   - Apply Kubernetes configurations.
   - Perform rolling updates and health checks.

### **5. Monitoring & Logging**
   - Integrate **Application Insights** (Azure), **CloudWatch** (AWS), or **Prometheus/Grafana**.
   - Enable centralized logging using ELK (Elasticsearch, Logstash, Kibana).

---

## **1️⃣ Azure DevOps Pipeline (AKS Deployment with Helm)**  
**Pipeline Configuration: `azure-pipelines.yml`**
```yaml
trigger:
  branches:
    include:
      - main

pool:
  vmImage: 'ubuntu-latest'

steps:
  - task: UseDotNet@2
    inputs:
      packageType: 'sdk'
      version: '8.0.x'

  - script: dotnet restore
    displayName: 'Restore dependencies'

  - script: dotnet build --configuration Release
    displayName: 'Build solution'

  - script: dotnet test --no-restore --verbosity normal
    displayName: 'Run unit tests'

  - task: Docker@2
    inputs:
      command: buildAndPush
      repository: myacr.azurecr.io/my-api
      dockerfile: 'Dockerfile'
      containerRegistry: 'MyAzureContainerRegistryServiceConnection'
      tags: latest

  - script: |
      az aks get-credentials --resource-group my-rg --name my-aks-cluster
      helm upgrade --install my-api ./helm --values helm/values.yaml
    displayName: 'Deploy to AKS'
```
### **Flow**
1. **Checkout Code** from Azure Repos.
2. **Restore, Build, and Test** .NET 8 API.
3. **Containerize the API** and push to **Azure Container Registry (ACR)**.
4. **Deploy to AKS using Helm**.

---

## **2️⃣ Jenkins Pipeline (AWS EKS Deployment with Helm)**
**Pipeline Configuration: `Jenkinsfile`**
```groovy
pipeline {
    agent any

    environment {
        AWS_ACCOUNT_ID = '123456789012'
        AWS_REGION = 'us-east-1'
        ECR_REPOSITORY = 'my-eks-repo'
        IMAGE_TAG = 'latest'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/my-repo.git'
            }
        }

        stage('Build & Test') {
            steps {
                sh 'dotnet restore'
                sh 'dotnet build --configuration Release'
                sh 'dotnet test --no-restore --verbosity normal'
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    sh "aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com"
                    sh "docker build -t $ECR_REPOSITORY:$IMAGE_TAG ."
                    sh "docker tag $ECR_REPOSITORY:$IMAGE_TAG $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/$ECR_REPOSITORY:$IMAGE_TAG"
                    sh "docker push $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/$ECR_REPOSITORY:$IMAGE_TAG"
                }
            }
        }

        stage('Deploy to EKS') {
            steps {
                sh "aws eks update-kubeconfig --region $AWS_REGION --name my-eks-cluster"
                sh "helm upgrade --install my-api ./helm --values helm/values.yaml"
            }
        }
    }
}
```
### **Flow**
1. **Checkout Code** from GitHub.
2. **Build & Test the .NET 8 API**.
3. **Push Docker Image** to AWS ECR.
4. **Deploy to AWS EKS** using Helm.

---

## **3️⃣ GitHub Actions Pipeline (Self-Managed Kubernetes with Helm)**
**Workflow Configuration: `.github/workflows/deploy.yml`**
```yaml
name: CI/CD for .NET 8 API

on:
  push:
    branches:
      - main

jobs:
  build-test-deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout Code
      uses: actions/checkout@v3

    - name: Setup .NET
      uses: actions/setup-dotnet@v3
      with:
        dotnet-version: '8.0.x'

    - name: Restore Dependencies
      run: dotnet restore

    - name: Build Solution
      run: dotnet build --configuration Release --no-restore

    - name: Run Tests
      run: dotnet test --no-restore --verbosity normal

    - name: Build Docker Image
      run: |
        docker build -t myregistry/my-api:latest .
        docker tag myregistry/my-api:latest myregistry/my-api:${{ github.sha }}

    - name: Push Docker Image
      run: |
        echo "${{ secrets.DOCKER_PASSWORD }}" | docker login -u "${{ secrets.DOCKER_USERNAME }}" --password-stdin
        docker push myregistry/my-api:latest
        docker push myregistry/my-api:${{ github.sha }}

    - name: Deploy to Kubernetes
      run: |
        kubectl config set-cluster my-cluster --server=${{ secrets.KUBE_SERVER }}
        kubectl config set-credentials admin --token=${{ secrets.KUBE_TOKEN }}
        kubectl config set-context my-context --cluster=my-cluster --user=admin
        kubectl config use-context my-context
        helm upgrade --install my-api ./helm --values helm/values.yaml
```
### **Flow**
1. **Triggers on Push** to the `main` branch.
2. **Builds & Tests** the .NET 8 API.
3. **Builds & Pushes Docker Image** to a registry.
4. **Deploys to Kubernetes** using Helm.

---

# **Comparison: Azure DevOps vs. Jenkins vs. GitHub Actions**
| Feature | Azure DevOps | Jenkins | GitHub Actions |
|---------|-------------|---------|---------------|
| **Ease of Setup** | Easy | Complex | Moderate |
| **Built-in Support for .NET** | Yes | Requires Plugins | Yes |
| **Security & Permissions** | RBAC (Azure AD) | Role-based | Fine-grained control |
| **Integration with Cloud** | Best with Azure | Best with AWS | Works with all |
| **Scalability** | High | High | Medium |
| **Kubernetes Deployment** | AKS-native | EKS-native | Works with any cluster |

---

# **Conclusion**
- **Use Azure DevOps** if working with **AKS** and require deep Azure integration.
- **Use Jenkins** for large-scale **AWS EKS deployments** with flexibility.
- **Use GitHub Actions** for **hybrid/self-managed Kubernetes clusters**.

