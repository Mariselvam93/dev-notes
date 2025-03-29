## **TeamCity**
## **What is TeamCity?**  
[TeamCity](https://www.jetbrains.com/teamcity/) is a **Continuous Integration (CI) and Continuous Deployment (CD) tool** developed by **JetBrains**. It helps automate the **build, test, and deployment** processes for software projects. TeamCity is used to:  

✅ **Run automated builds and tests**  
✅ **Integrate with various VCS (GitHub, Bitbucket, GitLab, etc.)**  
✅ **Deploy applications to multiple environments (AWS, Kubernetes, etc.)**  
✅ **Monitor build pipelines and track failures**  

---

## **🚀 Key Features of TeamCity**  

| Feature | Description |
|---------|------------|
| **CI/CD Automation** | Automates builds, tests, and deployments across environments. |
| **Parallel Builds** | Runs multiple builds in parallel to speed up execution. |
| **VCS Integration** | Supports GitHub, Bitbucket, GitLab, and other version control systems. |
| **Artifact Management** | Stores and retrieves artifacts for deployment. |
| **Custom Build Pipelines** | Allows custom workflows using Build Chains. |
| **Container & Cloud Support** | Integrates with Docker, Kubernetes, AWS, and Azure. |
| **Agent Management** | Uses build agents to distribute workloads efficiently. |
| **Infrastructure as Code** | Supports **Terraform, AWS CloudFormation**, and Kubernetes Helm. |
| **Notifications & Alerts** | Slack, email, or webhook notifications for build statuses. |
| **Access Control** | Role-based permissions and LDAP/Active Directory support. |
| **Test Automation & Reports** | Supports JUnit, NUnit, MSTest, and more. |

---

## **🛑 Missing Features / Limitations of TeamCity**
| **Limitation** | **Details** |
|---------------|------------|
| **UI Complexity** | The web UI can be complex for beginners. |
| **Limited Free Tier** | The free version supports only 3 build configurations. |
| **Resource Intensive** | Requires high CPU/RAM for large projects. |
| **Limited Containerization Support** | Works with Docker but lacks built-in Kubernetes-native CI/CD (compared to ArgoCD or GitHub Actions). |
| **Complex Agent Setup** | Setting up build agents across multiple environments requires manual effort. |
| **No Built-in Secret Management** | Does not natively manage secrets (requires HashiCorp Vault, AWS Secrets Manager, etc.). |
| **Longer Learning Curve** | Compared to GitHub Actions or GitLab CI/CD, it takes longer to master. |

---

## **🛠 When to Use TeamCity?**
✅ If you need **on-premise CI/CD** with enterprise security.  
✅ If your project requires **complex build dependencies and custom pipelines**.  
✅ If you need **parallel execution and scalability** for larger teams.  
---
# **Sample Usecase**
- **GitHub Actions (CI)** builds microservices and pushes artifacts to **JFrog Artifactory**.  
- **TeamCity (CD)** deploys services to **AWS ECS/EKS** across multiple environments (**DEV, Sandbox, Test, Stage, Prod**).  
- **AWS CloudFormation** manages infrastructure as code (IaC).  

---

# **🚀 Architecture Overview**  
1. **CI (Continuous Integration) using GitHub Actions**  
   - Code is pushed to GitHub.  
   - CI workflow runs tests, builds artifacts, and pushes them to **JFrog Artifactory**.  

2. **CD (Continuous Deployment) using TeamCity**  
   - TeamCity fetches artifacts from **JFrog**.  
   - Deploys microservices to AWS environments using **CloudFormation**.  

3. **AWS Infrastructure**  
   - Uses **AWS CloudFormation** to provision resources (ECS, EKS, RDS, ALB).  
   - Manages **Blue-Green Deployments** and **Canary Releases** for safer rollouts.  

---

# **🛠 Step 1: GitHub Actions (CI) - Build & Store Artifacts in JFrog**  
### **📌 GitHub Actions Workflow (`.github/workflows/build.yml`)**  
This workflow:  
✅ Checks out code  
✅ Builds .NET microservices  
✅ Runs tests  
✅ Packages and uploads artifacts to **JFrog Artifactory**  

```yaml
name: Build and Push to JFrog

on:
  push:
    branches:
      - main
      - release/*

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Set up .NET 8
        uses: actions/setup-dotnet@v3
        with:
          dotnet-version: '8.0.x'

      - name: Install Dependencies
        run: dotnet restore

      - name: Build the Application
        run: dotnet build --configuration Release --no-restore

      - name: Run Unit Tests
        run: dotnet test --configuration Release --no-restore

      - name: Package the Artifact
        run: |
          dotnet publish -c Release -o out
          tar -cvf my-app.tar out/

      - name: Push to JFrog Artifactory
        uses: jfrog/setup-jfrog-cli@v3
        with:
          version: latest
          
      - run: |
          jfrog rt ping
          jfrog rt upload my-app.tar my-repository/my-app/${{ github.sha }}.tar
          
        env:
          JFROG_CLI_OFFER_CONFIG: false
          JFROG_CLI_USER: ${{ secrets.JFROG_USERNAME }}
          JFROG_CLI_PASSWORD: ${{ secrets.JFROG_PASSWORD }}
          JFROG_CLI_URL: ${{ secrets.JFROG_URL }}
```

---

# **🛠 Step 2: TeamCity (CD) - Deploy Microservices to AWS**  
### **📌 TeamCity Deployment Pipeline**  
✅ **Triggers when a new artifact is available in JFrog**  
✅ **Deploys to environments in sequence: DEV → Sandbox → Test → Stage → Prod**  
✅ **Uses CloudFormation for infrastructure provisioning**  

### **1️⃣ Fetch Artifacts from JFrog**
```bash
jfrog rt download my-repository/my-app/${BUILD_NUMBER}.tar
tar -xvf my-app.tar -C /app
```

### **2️⃣ Deploy to AWS Using CloudFormation**
Each environment has a separate stack (e.g., `my-app-dev`, `my-app-prod`).

```bash
export ENV=dev
aws cloudformation deploy --template-file infra.yml --stack-name my-app-$ENV \
  --parameter-overrides Environment=$ENV ImageVersion=${BUILD_NUMBER}
```

---

# **🛠 Step 3: CloudFormation for Infrastructure as Code (IaC)**  
This **CloudFormation template** provisions:  
✅ **AWS ECS Cluster** for microservices  
✅ **Task Definition for containers**  
✅ **Service with Fargate deployment**  
✅ **Auto-scaling & Load Balancing**  

### **📌 `infra.yml` (CloudFormation Template)**  
```yaml
AWSTemplateFormatVersion: '2010-09-09'
Resources:
  ECSCluster:
    Type: AWS::ECS::Cluster
    Properties:
      ClusterName: !Sub "MyApp-${Environment}"

  Service:
    Type: AWS::ECS::Service
    Properties:
      Cluster: !Ref ECSCluster
      LaunchType: FARGATE
      DesiredCount: 2
      TaskDefinition: !Ref TaskDef

  TaskDef:
    Type: AWS::ECS::TaskDefinition
    Properties:
      Family: MyApp
      ContainerDefinitions:
        - Name: MyAppContainer
          Image: !Sub "123456789.dkr.ecr.us-east-1.amazonaws.com/my-app:${ImageVersion}"
          Memory: 512
          Cpu: 256
```

---

# **🛠 Step 4: Environment-based Deployment Strategy**  
| **Environment** | **Deployment Strategy** |
|----------------|-------------------------|
| **DEV**        | Automatic deployment on push |
| **Sandbox**    | Manual approval required |
| **Test**       | Requires validation before release |
| **Stage**      | Canary Deployment |
| **Prod**       | Blue-Green Deployment |

### **🛠 Deploy to Specific Environments**
```bash
export ENV=stage
aws cloudformation deploy --template-file infra.yml --stack-name my-app-$ENV \
  --parameter-overrides Environment=$ENV ImageVersion=${BUILD_NUMBER}
```

---

# **✅ Summary**
| **Step** | **Tool Used** | **Purpose** |
|----------|-------------|-------------|
| **CI (Build & Test)** | **GitHub Actions** | Builds .NET microservice & pushes to JFrog |
| **Artifact Repository** | **JFrog Artifactory** | Stores built application artifacts |
| **CD (Deployment)** | **TeamCity** | Fetches artifacts & deploys to AWS |
| **Infrastructure as Code** | **AWS CloudFormation** | Manages AWS infrastructure |
| **Container Deployment** | **AWS ECS (Fargate) / EKS** | Runs microservices in containers |

---
