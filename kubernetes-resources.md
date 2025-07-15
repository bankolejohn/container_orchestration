# **Working with Kubernetes Resources**  
## **Comprehensive Guide to YAML, Deployments, and Services**  

## **Project Overview**  
This project explores **Kubernetes resource management** using YAML configuration files. By the end, you will:  
- Understand **YAML syntax** for Kubernetes configurations.  
- Learn to **deploy applications** using Deployments and Services.  
- Master **declarative vs imperative** Kubernetes management.  
- Gain hands-on experience with **Minikube deployments**.  

---

## **Table of Contents**  
1. [Introduction to YAML](#introduction-to-yaml)  
2. [Deploying Applications in Kubernetes](#deploying-applications-in-kubernetes)  
3. [Working with Deployments](#working-with-deployments)  
4. [Working with Services](#working-with-services)  
5. [Hands-On: Deploying Nginx with YAML](#hands-on-deploying-nginx-with-yaml)  
6. [Troubleshooting](#troubleshooting)  
7. [Conclusion](#conclusion)  

---

## **Introduction to YAML**  
YAML (YAML Ain't Markup Language) is a human-readable data format used for Kubernetes configurations.  

### **Basic Structure**  
```yaml
apiVersion: v1  # Kubernetes API version
kind: Pod       # Resource type
metadata:       # Identifying info
  name: my-pod
spec:           # Desired state
  containers:
  - name: nginx
    image: nginx:latest
```

### **Key YAML Concepts**  
| **Concept**       | **Example** |  
|-------------------|------------|  
| **Scalars**       | `port: 80` |  
| **Lists**         | `- apple`<br>`- banana` |  
| **Maps**          | `env:<br>  name: value` |  
| **Multiline**     | `description: \|`<br>`  Line 1`<br>`  Line 2` |  
| **Comments**      | `# This is a comment` |  

---

## **Deploying Applications in Kubernetes**  

### **1. Imperative Approach (Quick Start)**  
```bash
# Create a Deployment
kubectl create deployment hello-minikube --image=kicbase/echo-server:1.0

# Expose as a Service
kubectl expose deployment hello-minikube --type=NodePort --port=8080

# Access the Service
minikube service hello-minikube
```

### **2. Declarative Approach (Recommended)**  
- Define resources in **YAML files** (e.g., `deployment.yaml`, `service.yaml`).  
- Apply with:  
  ```bash
  kubectl apply -f deployment.yaml
  ```

---

## **Working with Deployments**  
Deployments manage **Pod lifecycles** and enable:  
✔ **Scaling** (replicas)  
✔ **Rolling updates**  
✔ **Self-healing**  

### **Example: `nginx-deployment.yaml`**  
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```

### **Key Commands**  
| **Command** | **Purpose** |  
|-------------|------------|  
| `kubectl get deployments` | List Deployments |  
| `kubectl describe deployment <name>` | Inspect details |  
| `kubectl scale deployment <name> --replicas=5` | Scale Pods |  

---

## **Working with Services**  
Services provide **stable network access** to Pods.  

### **Service Types**  
| **Type**       | **Use Case** |  
|----------------|-------------|  
| **ClusterIP**  | Internal cluster access (default) |  
| **NodePort**   | External access via node IP |  
| **LoadBalancer**| Cloud-provided external IP |  

### **Example: `nginx-service.yaml`**  
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: NodePort
```

### **Key Commands**  
| **Command** | **Purpose** |  
|-------------|------------|  
| `kubectl get services` | List Services |  
| `minikube service <name> --url` | Get Service URL |  

---

## **Hands-On: Deploying Nginx with YAML**  

### **1. Create the Deployment**  
```bash
kubectl apply -f nginx-deployment.yaml
```

### **2. Create the Service**  
```bash
kubectl apply -f nginx-service.yaml
```

### **3. Verify Resources**  
```bash
kubectl get pods,deployments,services
```

### **4. Access the Application**  
```bash
minikube service nginx-service --url
```
Open the URL in a browser to see Nginx.  

---

## **Troubleshooting**  

| **Issue** | **Solution** |  
|-----------|-------------|  
| **ImagePullBackOff** | Check image name/tag in YAML. |  
| **CrashLoopBackOff** | View logs: `kubectl logs <pod>` |  
| **Service Unreachable** | Verify `selector` matches Pod labels. |  

---

## **Conclusion**  
You’ve learned how to:  
✔ Write **Kubernetes YAML** configurations.  
✔ Deploy applications using **Deployments**.  
✔ Expose apps with **Services**.  

