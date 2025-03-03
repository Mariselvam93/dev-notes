Here's a **Helm chart** for your **e-commerce microservices architecture** using Kubernetes. This will automate the deployment of your **Product Service** with a **Deployment, Service, Ingress, ConfigMap, Secret, and Horizontal Pod Autoscaler (HPA).**

---

## **1️⃣ Helm Chart Directory Structure**
```
product-service-chart/
│── templates/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── ingress.yaml
│   ├── configmap.yaml
│   ├── secret.yaml
│   ├── hpa.yaml
│── Chart.yaml
│── values.yaml
│── .helmignore
```

---

## **2️⃣ `Chart.yaml` (Metadata for the Helm Chart)**
```yaml
apiVersion: v2
name: product-service
description: A Helm chart for deploying Product Service
type: application
version: 1.0.0
appVersion: "1.0.0"
```

---

## **3️⃣ `values.yaml` (Default Configurations)**
This file contains **all configurable values** used across the templates.

```yaml
replicaCount: 3

image:
  repository: myrepo/product-service
  tag: latest
  pullPolicy: Always

service:
  type: ClusterIP
  port: 80

ingress:
  enabled: true
  host: shop.example.com
  path: /products

configMap:
  DATABASE_URL: "postgres://db-user:password@database:5432/products"
  LOG_LEVEL: "info"

secret:
  DB_PASSWORD: "cGFzc3dvcmQ="  # base64 encoded password

hpa:
  enabled: true
  minReplicas: 2
  maxReplicas: 10
  targetCPUUtilizationPercentage: 50
```

---

## **4️⃣ Deployment Template (`templates/deployment.yaml`)**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Release.Name }}-product-service
spec:
  replicas: {{ .Values.replicaCount }}
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
        image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
        imagePullPolicy: {{ .Values.image.pullPolicy }}
        ports:
        - containerPort: 80
        env:
        - name: DATABASE_URL
          valueFrom:
            configMapKeyRef:
              name: {{ .Release.Name }}-config
              key: DATABASE_URL
        - name: DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: {{ .Release.Name }}-secret
              key: DB_PASSWORD
```

---

## **5️⃣ Service Template (`templates/service.yaml`)**
```yaml
apiVersion: v1
kind: Service
metadata:
  name: {{ .Release.Name }}-product-service
spec:
  type: {{ .Values.service.type }}
  selector:
    app: product-service
  ports:
    - protocol: TCP
      port: {{ .Values.service.port }}
      targetPort: 80
```

---

## **6️⃣ Ingress Template (`templates/ingress.yaml`)**
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: {{ .Release.Name }}-ingress
spec:
  rules:
  - host: {{ .Values.ingress.host }}
    http:
      paths:
      - path: {{ .Values.ingress.path }}
        pathType: Prefix
        backend:
          service:
            name: {{ .Release.Name }}-product-service
            port:
              number: 80
```

---

## **7️⃣ ConfigMap Template (`templates/configmap.yaml`)**
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}-config
data:
  DATABASE_URL: "{{ .Values.configMap.DATABASE_URL }}"
  LOG_LEVEL: "{{ .Values.configMap.LOG_LEVEL }}"
```

---

## **8️⃣ Secret Template (`templates/secret.yaml`)**
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: {{ .Release.Name }}-secret
type: Opaque
data:
  DB_PASSWORD: "{{ .Values.secret.DB_PASSWORD }}"
```

---

## **9️⃣ Horizontal Pod Autoscaler Template (`templates/hpa.yaml`)**
```yaml
{{- if .Values.hpa.enabled }}
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: {{ .Release.Name }}-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: {{ .Release.Name }}-product-service
  minReplicas: {{ .Values.hpa.minReplicas }}
  maxReplicas: {{ .Values.hpa.maxReplicas }}
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: {{ .Values.hpa.targetCPUUtilizationPercentage }}
{{- end }}
```

---

## **🔟 Install & Deploy the Helm Chart**
### **1️⃣ Package the Helm Chart**
```sh
helm package product-service-chart
```

### **2️⃣ Install the Chart**
```sh
helm install product-service ./product-service-chart
```

### **3️⃣ Check Deployment**
```sh
kubectl get pods
kubectl get svc
kubectl get ingress
```

### **4️⃣ Upgrade Chart**
If you make changes in `values.yaml`, upgrade the deployment:
```sh
helm upgrade product-service ./product-service-chart
```

---

## **🎯 Summary**
✅ Automates the Kubernetes deployment of **Product Service**  
✅ Supports **configuration management** via `values.yaml`  
✅ Securely stores **secrets** using Kubernetes Secrets  
✅ Implements **Ingress** for routing  
✅ Enables **autoscaling** with HPA  
