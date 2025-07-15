# **Working with Kubernetes Pods**  
## **Comprehensive Guide to Pods and Containers in Kubernetes**  

## **Project Overview**  
This project explores **Kubernetes Pods**, the smallest deployable units in Kubernetes that host one or more containers. By the end of this guide, you will:  
- Understand **what Pods are** and their role in Kubernetes.  
- Learn how to **create, inspect, and manage Pods** using `kubectl`.  
- Explore the relationship between **Pods and Containers**.  
- Gain hands-on experience with **Pod definitions in YAML**.  

---

## **Table of Contents**  
1. [Introduction to Kubernetes Pods](#introduction-to-kubernetes-pods)  
2. [Creating and Managing Pods](#creating-and-managing-pods)  
3. [Inspecting Pod Details](#inspecting-pod-details)  
4. [Containers in Kubernetes](#containers-in-kubernetes)  
5. [Defining Pods with YAML](#defining-pods-with-yaml)  
6. [Common Pod Operations](#common-pod-operations)  
7. [Troubleshooting Pod Issues](#troubleshooting-pod-issues)  
8. [Conclusion](#conclusion)  

---

## **Introduction to Kubernetes Pods**  
A **Pod** is the smallest and simplest Kubernetes object. It represents a single instance of a running process in a cluster and can contain:  
✔ **One or more containers** (usually tightly coupled).  
✔ **Shared storage and network** (containers in a Pod share the same IP and ports).  
✔ **Configuration options** (e.g., environment variables, volumes).  

### **Key Characteristics of Pods**  
- **Ephemeral**: Pods are created and destroyed dynamically.  
- **Atomic Unit**: Kubernetes manages Pods, not individual containers.  
- **Scaling**: Done via **Deployments** (not by manually creating Pods).  

---

## **Creating and Managing Pods**  

### **1. List All Pods**  
```bash
kubectl get pods -A  # -A shows pods in all namespaces
```
**Example Output:**  
```
NAMESPACE     NAME                               READY   STATUS    RESTARTS   AGE  
default       nginx-pod                          1/1     Running   0          5m  
kube-system   coredns-5c98db65d4-2q6zv           1/1     Running   0          10m  
```

### **2. Create a Pod (Imperative Command)**  
```bash
kubectl run nginx-pod --image=nginx
```
- Creates an **NGINX Pod** using the latest `nginx` image.  

### **3. Delete a Pod**  
```bash
kubectl delete pod nginx-pod
```
- Removes the Pod (Kubernetes may recreate it if managed by a **Deployment**).  

---

## **Inspecting Pod Details**  

### **1. Describe a Pod**  
```bash
kubectl describe pod <pod-name>
```
**Example:**  
```bash
kubectl describe pod nginx-pod
```
**Output Includes:**  
✔ **Pod status** (Running, Pending, CrashLoopBackOff).  
✔ **Container details** (image, ports, environment variables).  
✔ **Events log** (helpful for debugging).  

### **2. View Pod Logs**  
```bash
kubectl logs <pod-name>
```
- Useful for **debugging container issues**.  

### **3. Execute Commands Inside a Pod**  
```bash
kubectl exec -it <pod-name> -- /bin/bash
```
- Opens an **interactive shell** inside the Pod.  

---

## **Containers in Kubernetes**  
Containers (e.g., Docker) run inside Pods. Key concepts:  

### **1. Single-Container Pod**  
- Most common use case (e.g., `nginx`, `redis`).  

### **2. Multi-Container Pods**  
- Used when containers **need to share resources** (e.g., a web server + log processor).  

### **3. Sidecar Pattern**  
- A helper container **assists the main container** (e.g., logging, monitoring).  

---

## **Defining Pods with YAML**  
Best practice: Define Pods using **declarative YAML** files.  

### **Example: `nginx-pod.yaml`**  
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
```

### **Apply the YAML File**  
```bash
kubectl apply -f nginx-pod.yaml
```

---

## **Common Pod Operations**  

| **Command** | **Description** |  
|-------------|----------------|  
| `kubectl get pods` | List all Pods |  
| `kubectl logs <pod>` | View Pod logs |  
| `kubectl exec -it <pod> -- <cmd>` | Run a command in a Pod |  
| `kubectl port-forward <pod> 8080:80` | Forward Pod port to localhost |  
| `kubectl top pod` | Monitor Pod resource usage |  

---

## **Troubleshooting Pod Issues**  

| **Issue** | **Solution** |  
|-----------|-------------|  
| **Pod "Pending"** | Check `kubectl describe pod` for resource constraints. |  
| **CrashLoopBackOff** | Inspect logs (`kubectl logs <pod>`). |  
| **ImagePullBackOff** | Verify the container image exists and is accessible. |  
| **Container Not Starting** | Check `kubectl describe pod` for errors. |  

---

## **Conclusion**  
You now understand **how Pods work** in Kubernetes and how to manage them effectively.  

### **Key Takeaways**  
✔ Pods are the **smallest deployable units** in Kubernetes.  
✔ Use `kubectl get pods` and `describe pod` for **inspection**.  
✔ **Multi-container Pods** are useful for tightly coupled services.  
✔ **YAML definitions** are the recommended way to manage Pods.  

## Screenshots

<img width="506" height="29" alt="Snipaste_2025-07-15_11-33-56" src="https://github.com/user-attachments/assets/8ae8a78a-e3ba-4948-a4a0-b1853a3bd26e" />


<img width="449" height="23" alt="Snipaste_2025-07-15_11-32-53" src="https://github.com/user-attachments/assets/edc7abe0-7f4e-46c2-bbe3-e65afd47458b" />
