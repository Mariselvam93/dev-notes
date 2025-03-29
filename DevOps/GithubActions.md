# **🔹 GitHub Actions Deep Dive**

GitHub Actions is a **powerful CI/CD** (Continuous Integration & Continuous Deployment) service built into GitHub. It allows us to automate software workflows directly in repositories.

---

## **🔹 How GitHub Actions Works**

A **GitHub Actions Workflow** consists of:  
✅ **Triggers (Events):** Actions start based on repository events (push, PR, schedule, etc.).  
✅ **Jobs:** Define a set of steps executed in sequence or parallel.  
✅ **Steps:** Individual tasks in a job (e.g., checkout, build, test, deploy).  
✅ **Runners:** Machines that execute workflows (GitHub-hosted or self-hosted).  
✅ **Secrets & Environment Variables:** Securely store credentials & configurations.

---

### **1.1 Workflows**
- A **workflow** is a YAML configuration file stored in `.github/workflows/`.  
- It defines the automation process and is triggered by events like `push`, `pull_request`, or `schedule`.

Example:
```yaml
name: My CI Workflow
on: [push, pull_request]
```

---

## **2. Runners (`where to run`)**
A **runner** is the execution environment for GitHub Actions.

### **2.1 Types of Runners**
#### **GitHub-Hosted Runners (Default)**
GitHub provides pre-configured virtual machines:
- `ubuntu-latest`
- `windows-latest`
- `macos-latest`

Example:
```yaml
jobs:
  build:
    runs-on: ubuntu-latest
```

#### **Self-Hosted Runners**
- Custom, dedicated servers.
- Useful for running workflows inside a private network.
- Needs to be manually registered.

Example:
```yaml
jobs:
  deploy:
    runs-on: self-hosted
```

---

## **3. Jobs (`what to execute`)**
- **Jobs** are independent execution units within a workflow.
- Jobs run **in parallel by default** unless configured with `needs`.

Example:
```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v4
  test:
    needs: build  # Runs only after 'build' job
    runs-on: ubuntu-latest
    steps:
      - run: echo "Running tests..."
```

### **3.1 Job Dependencies (`needs`)**
- Control job execution order.
- `needs` ensures a job runs **only if its dependent jobs succeed**.

Example:
```yaml
jobs:
  build:
    runs-on: ubuntu-latest
  deploy:
    needs: build  # Deploy runs only if build succeeds
    runs-on: ubuntu-latest
```

---

## **4. Steps (`how to execute tasks inside a job`)**
- A **step** is a command or action executed within a job.
- Steps run **sequentially** within a job.

### **4.1 Types of Steps**
#### **Running Shell Commands**
```yaml
steps:
  - name: Print message
    run: echo "Hello, GitHub Actions!"
```

#### **Using Pre-Built Actions**
```yaml
steps:
  - name: Checkout Repository
    uses: actions/checkout@v4
  - name: Set up Node.js
    uses: actions/setup-node@v3
    with:
      node-version: 18
```

#### **Using Environment Variables**
```yaml
steps:
  - name: Use an environment variable
    run: echo "Environment: ${{ env.ENVIRONMENT }}"
    env:
      ENVIRONMENT: production
```

---

## **5. Execution Conditions (`when to run`)**
- Use `if:` to conditionally run jobs or steps.

### **5.1 Run Only on Main Branch**
```yaml
jobs:
  deploy:
    if: github.ref == 'refs/heads/main'
```

### **5.2 Skip Jobs on Pull Requests**
```yaml
jobs:
  build:
    if: github.event_name != 'pull_request'
```

---

## **6. Actions (`what to do`)**
Actions are reusable components that encapsulate logic.

### **6.1 Using Marketplace Actions**
```yaml
- name: Upload Artifact
  uses: actions/upload-artifact@v3
  with:
    name: my-artifact
    path: dist/
```

### **6.2 Creating Custom Actions**
#### **Shell Script Action**
```yaml
runs:
  using: "composite"
  steps:
    - run: echo "Custom Action Running..."
      shell: bash
```

---

## **7. Advanced Features**
### **7.1 Matrix Builds**
Run a job across multiple OS versions:
```yaml
strategy:
  matrix:
    os: [ubuntu-latest, windows-latest]
jobs:
  test:
    runs-on: ${{ matrix.os }}
```

### **7.2 Caching Dependencies**
```yaml
- name: Cache NuGet Packages
  uses: actions/cache@v3
  with:
    path: ~/.nuget/packages
    key: nuget-${{ runner.os }}-${{ hashFiles('**/packages.lock.json') }}
```

### **7.3 Artifacts (Sharing Files Between Jobs)**
```yaml
- name: Upload Artifact
  uses: actions/upload-artifact@v3
  with:
    name: build-output
    path: bin/Release/
```

---

# **🔹 Implementing GitHub Actions in a .NET 8 API**
We'll automate the following for a **.NET 8 Web API**:  
1️⃣ **Build & Test** → Compile and run tests  
2️⃣ **Cache Dependencies** → Optimize builds  
3️⃣ **Static Code Analysis** → Enforce quality  
4️⃣ **Publish & Store Artifacts** → Upload build output  
5️⃣ **Docker Build & Push** → Containerize the app  
6️⃣ **Deploy to AWS ECS & Kubernetes** → Automate cloud deployment  

---

## **🔹 Step 1: Create a .NET 8 Web API**
```sh
dotnet new webapi -n SampleApi
cd SampleApi
dotnet run
```
Create a `.gitignore` file (important for GitHub Actions):
```sh
dotnet new gitignore
```
Initialize a GitHub repository:
```sh
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin <your-github-repo-url>
git push -u origin main
```
---

## **🔹 Step 2: Create GitHub Actions Workflow**
Inside your **GitHub repository**, create the folder `.github/workflows/ci-cd.yml`.

### **📌 Full .NET 8 CI/CD Workflow (`ci-cd.yml`)**
```yaml
name: .NET 8 CI/CD Pipeline

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Set up .NET 8
        uses: actions/setup-dotnet@v3
        with:
          dotnet-version: 8.0.x

      - name: Restore Dependencies
        run: dotnet restore

      - name: Cache Dependencies
        uses: actions/cache@v3
        with:
          path: ~/.nuget/packages
          key: nuget-${{ runner.os }}-${{ hashFiles('**/packages.lock.json') }}
          restore-keys: |
            nuget-${{ runner.os }}-

      - name: Build the Solution
        run: dotnet build --configuration Release --no-restore

      - name: Run Tests
        run: dotnet test --configuration Release --no-build --verbosity normal

  dockerize:
    needs: build
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Build Docker Image
        run: |
          docker build -t sample-api:${{ github.sha }} .

      - name: Log in to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Push Docker Image
        run: |
          docker tag sample-api:${{ github.sha }} <your-dockerhub-username>/sample-api:latest
          docker push <your-dockerhub-username>/sample-api:latest

  deploy:
    needs: dockerize
    runs-on: ubuntu-latest

    steps:
      - name: Deploy to AWS ECS
        uses: aws-actions/amazon-ecs-deploy-task-definition@v1
        with:
          cluster: my-cluster
          service: sample-api-service
          task-definition: task-definition.json
```
---

# **🔹 Deep Dive into the Workflow**
### ✅ **1. `on:` Events (Triggers)**
We trigger this workflow on:
- `push` → Runs when new code is pushed.
- `pull_request` → Runs on PRs.

### ✅ **2. `jobs:` (Stages of the Pipeline)**
- `build` → Compiles & runs tests.
- `dockerize` → Builds a Docker image & pushes it to Docker Hub.
- `deploy` → Deploys the app to AWS ECS.

---

# **🔹 Step 3: Deployment Options**
## **🚀 Option 1: Deploying to AWS ECS**
AWS **Elastic Container Service (ECS)** runs containers in a managed cluster.

### **Prerequisites:**
1. **Create an ECS Cluster**
```sh
aws ecs create-cluster --cluster-name my-cluster
```
2. **Create a Task Definition (`task-definition.json`)**
```json
{
  "family": "sample-api-task",
  "containerDefinitions": [
    {
      "name": "sample-api",
      "image": "<your-dockerhub-username>/sample-api:latest",
      "cpu": 256,
      "memory": 512,
      "portMappings": [
        {
          "containerPort": 80,
          "hostPort": 80
        }
      ]
    }
  ]
}
```
3. **Register the Task Definition**
```sh
aws ecs register-task-definition --cli-input-json file://task-definition.json
```
4. **Create an ECS Service**
```sh
aws ecs create-service \
  --cluster my-cluster \
  --service-name sample-api-service \
  --task-definition sample-api-task \
  --desired-count 1 \
  --launch-type FARGATE
```

---

## **🚀 Option 2: Deploying to Kubernetes**
### **Step 1: Create a Deployment YAML (`deployment.yml`)**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: sample-api
spec:
  replicas: 2
  selector:
    matchLabels:
      app: sample-api
  template:
    metadata:
      labels:
        app: sample-api
    spec:
      containers:
      - name: sample-api
        image: <your-dockerhub-username>/sample-api:latest
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: sample-api-service
spec:
  selector:
    app: sample-api
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: LoadBalancer
```
### **Step 2: Apply Deployment**
```sh
kubectl apply -f deployment.yml
```

---

# **🔹 Best Practices for GitHub Actions**
✅ **Use Caching** → Speeds up workflows (`actions/cache@v3`)  
✅ **Parallel Execution** → Use `needs:` to run jobs concurrently  
✅ **Secure Secrets** → Store credentials in `Secrets` (`Settings > Secrets`)  
✅ **Use Matrix Builds** → Test against multiple environments  
✅ **Reusable Workflows** → Call common workflows from different repositories  

---

# **🔹 Final Thoughts**
- **GitHub Actions automates CI/CD**, making builds and deployments **faster & efficient**.
- **Deploy to AWS ECS** for a **serverless** containerized solution.
- **Use Kubernetes** for **scalability & portability**.
- **Secure workflows with best practices** (caching, secrets, parallelism).

---
# **🚀 DevSecOps: Security in DevOps Pipeline**  

### **🔹 What is DevSecOps?**
DevSecOps integrates **security at every stage** of the **software development lifecycle (SDLC)**. Instead of handling security at the end, DevSecOps ensures **continuous security checks** in **development, testing, deployment, and operations**.

📌 **DevSecOps = Development + Security + Operations**  
📌 **"Shift-Left" Security Approach** → Embed security **early** in CI/CD  
📌 **Automation** → Secure coding, vulnerability scanning, compliance enforcement  
📌 **Collaboration** → Dev, Sec, and Ops teams work **together**  

---

# **🔹 DevSecOps Pipeline Overview**
A **secure CI/CD pipeline** ensures that every commit is **scanned, tested, and verified** before deployment.

### **🔗 DevSecOps Workflow**
1️⃣ **Developer Commits Code** → Code analysis (SAST)  
2️⃣ **Build & Dependency Scanning** → Check for vulnerabilities (SCA)  
3️⃣ **Unit & Security Tests** → Automated tests (DAST, SAST)  
4️⃣ **Container Security** → Scan images for vulnerabilities  
5️⃣ **Deployment with Compliance** → Secure secrets & configurations  
6️⃣ **Runtime Security Monitoring** → Logs, anomaly detection  

---

# **🔹 DevSecOps Toolchain**
| **Stage** | **Security Practice** | **Tools** |
|-----------|----------------|---------|
| **Code** | Secure coding practices | SonarQube, Checkmarx, ESLint, Prettier |
| **Build** | Dependency vulnerability scanning | Snyk, OWASP Dependency-Check |
| **Test** | SAST (Static Analysis) | SonarQube, Fortify, Bandit (Python) |
| **Container** | Image scanning | Trivy, AquaSec, Clair |
| **Secrets** | Secrets detection | GitLeaks, AWS Secrets Manager |
| **Deploy** | Infrastructure security | Terraform Sentinel, Open Policy Agent |
| **Monitor** | Runtime security | Falco, Sysdig, AWS GuardDuty |

---

# **🔹 DevSecOps in Action: .NET 8 API + GitHub Actions + AWS**
Let’s implement **DevSecOps** in a **.NET 8 API** pipeline with security checks and AWS deployment.

---

## **🛠 1️⃣ Secure Code with SonarQube**
SonarQube performs **static code analysis (SAST)** to detect vulnerabilities.

📌 **Setup SonarQube in GitHub Actions**
```yaml
- name: SonarQube Scan
  uses: sonarsource/sonarqube-scan-action@master
  env:
    SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
    SONAR_HOST_URL: "https://sonarqube.example.com"
```

---

## **🛠 2️⃣ Dependency Scanning with OWASP Dependency-Check**
Identifies vulnerabilities in .NET NuGet packages.

📌 **Install OWASP Dependency-Check**
```sh
dotnet tool install --global OWASP.DependencyCheck
```
📌 **Run Scan**
```sh
dependency-check --project "ECommerceAPI" --scan .
```

---

## **🛠 3️⃣ Container Security with Trivy**
Trivy scans **Docker images** for vulnerabilities.

📌 **GitHub Action for Trivy**
```yaml
- name: Run Trivy Scan
  uses: aquasecurity/trivy-action@master
  with:
    image-ref: "myrepo/ecommerce-api:latest"
    format: "table"
```

---

## **🛠 4️⃣ Secrets Detection with GitLeaks**
Scans for **hardcoded secrets** in the codebase.

📌 **GitHub Action for GitLeaks**
```yaml
- name: Run GitLeaks Scan
  uses: zricethezav/gitleaks-action@v2
```

---

## **🛠 5️⃣ Secure Infrastructure with Terraform Sentinel**
📌 **Prevent deployment if security policies fail**
```hcl
policy "enforce-iam-role" {
  enforcement_level = "hard-mandatory"
  condition = tfplan.resource_changes[*].address contains "aws_iam_role"
}
```

---

## **🛠 6️⃣ AWS Security Best Practices**
🔹 **IAM Roles** → Use least privilege for services  
🔹 **Secrets Manager** → Store API keys securely  
🔹 **CloudWatch & GuardDuty** → Monitor logs & detect threats  
🔹 **WAF & Shield** → Protect APIs from DDoS attacks  

---

# **🔹 Deployment to AWS ECS with Security**
Deploy **secure containers** to AWS ECS.

📌 **ECS Task Definition (`task-definition.json`)**
```json
{
  "containerDefinitions": [
    {
      "name": "ecommerce-api",
      "image": "myrepo/ecommerce-api:latest",
      "cpu": 256,
      "memory": 512,
      "logConfiguration": {
        "logDriver": "awslogs",
        "options": {
          "awslogs-group": "/ecs/ecommerce-api",
          "awslogs-region": "us-east-1"
        }
      }
    }
  ]
}
```
📌 **Deploy with AWS CLI**
```sh
aws ecs update-service --cluster ecommerce-cluster --service ecommerce-api --force-new-deployment
```

---

# **🔹 Runtime Security Monitoring with Falco**
Falco detects **suspicious activity** in running containers.

📌 **Install Falco**
```sh
helm install falco falcosecurity/falco
```
📌 **Monitor Logs**
```sh
kubectl logs -l app=falco
```

---

# **🔹 DevSecOps Best Practices**
✅ **Shift Left Security** → Run scans **before deployment**  
✅ **Automate Security Checks** → Use **CI/CD pipelines**  
✅ **Enforce Policies** → Use Terraform Sentinel, Open Policy Agent  
✅ **Use Least Privilege** → Limit **IAM roles & API keys**  
✅ **Encrypt Data** → Use **AWS KMS & TLS for API communication**  
✅ **Monitor Continuously** → Use **CloudWatch, GuardDuty, Falco**  

---

# **🚀 Conclusion**
DevSecOps ensures **continuous security integration** in the DevOps lifecycle.  
By implementing **SAST, DAST, container security, and monitoring**, your application stays **secure, compliant, and resilient**! 🔐✨  
