# **Networking in Kubernetes**  
## **Comprehensive Guide to Pod Networking, Services, and Ingress**  

## **Project Overview**  
This project explores **Kubernetes networking**, focusing on how Pods, Services, and Ingress enable communication within and outside the cluster. By the end, you will:  
- Understand **Pod-to-Pod communication** and shared network namespaces.  
- Learn how **Services** provide stable network endpoints.  
- Configure **Ingress** for external access.  
- Implement **Network Policies** for security.  

---

## **Table of Contents**  
1. [Kubernetes Networking Fundamentals](#kubernetes-networking-fundamentals)  
2. [Pod Networking](#pod-networking)  
3. [Service Networking](#service-networking)  
4. [Ingress Controllers](#ingress-controllers)  
5. [Network Policies](#network-policies)  
6. [Hands-On: Multi-Container Pod](#hands-on-multi-container-pod)  
7. [Troubleshooting](#troubleshooting)  
8. [Conclusion](#conclusion)  

---

## **Kubernetes Networking Fundamentals**  
Kubernetes networking enables:  
✔ **Pod-to-Pod communication** (cross-node)  
✔ **Service discovery** (via DNS)  
✔ **External access** (via Services/Ingress)  

### **Key Concepts**  
| **Component** | **Purpose** |  
|--------------|------------|  
| **Pod Network** | Each Pod gets a unique IP; containers share network namespace. |  
| **Service** | Stable IP/DNS name for a group of Pods. |  
| **Ingress** | Manages external HTTP(S) traffic routing. |  
| **CNI Plugins** | Implements networking (e.g., Calico, Flannel). |  

---

## **Pod Networking**  
Containers in the **same Pod**:  
- Share the **same IP** and **port space**.  
- Communicate via `localhost`.  

### **Example: Multi-Container Pod**  
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: multi-container-pod
spec:
  containers:
  - name: nginx
    image: nginx:alpine
  - name: logger
    image: busybox
    command: ["/bin/sh", "-c", "while true; do echo 'Hello' >> /usr/share/nginx/html/logs.txt; sleep 5; done"]
```
**Key Points**:  
- Both containers share `/usr/share/nginx/html`.  
- `logger` can access Nginx via `http://localhost`.  

---

## **Service Networking**  
Services provide **stable endpoints** for Pods.  

### **Service Types**  
| **Type** | **Use Case** |  
|----------|-------------|  
| **ClusterIP** | Internal cluster access (default). |  
| **NodePort** | Exposes Service on a static port (30000-32767). |  
| **LoadBalancer** | Cloud-provided external IP. |  

### **Example: NodePort Service**  
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
**Access the Service**:  
```bash
minikube service nginx-service --url
```

---

## **Ingress Controllers**  
Ingress manages **external HTTP(S) traffic**.  

### **Example: Ingress Resource**  
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: nginx-ingress
spec:
  rules:
  - host: myapp.test
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: nginx-service
            port:
              number: 80
```
**Prerequisite**:  
- Install an **Ingress Controller** (e.g., Nginx Ingress):  
  ```bash
  minikube addons enable ingress
  ```

---

## **Network Policies**  
Restrict Pod communication for security.  

### **Example: Deny All Traffic**  
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-all
spec:
  podSelector: {}
  policyTypes:
  - Ingress
  - Egress
```
**Allow Specific Traffic**:  
```yaml
spec:
  podSelector:
    matchLabels:
      app: nginx
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: client
```

---

## **Hands-On: Multi-Container Pod**  

### **1. Deploy the Pod**  
```bash
kubectl apply -f multi-container-pod.yaml
```

### **2. Verify Shared Networking**  
```bash
# Access the Nginx container
kubectl exec -it multi-container-pod -c nginx -- curl localhost

# Check logs from the logger container
kubectl logs multi-container-pod -c logger
```

### **3. Test Communication**  
```bash
# From the logger container
kubectl exec -it multi-container-pod -c logger -- wget -qO- http://localhost
```
**Expected Output**: Nginx default page.  

---

## **Troubleshooting**  

| **Issue** | **Solution** |  
|-----------|-------------|  
| **Pods can’t communicate** | Check CNI plugin and Network Policies. |  
| **Service unreachable** | Verify `selector` matches Pod labels. |  
| **Ingress not working** | Ensure Ingress Controller is running. |  

---

## **Conclusion**  
You’ve learned:  
✔ How **Pods share networks** and communicate.  
✔ How **Services** provide stable access.  
✔ How **Ingress** routes external traffic.  

## Screenshots

<img width="677" height="27" alt="Snipaste_2025-07-15_13-29-02" src="https://github.com/user-attachments/assets/3f111e2f-2b3b-4fb3-ae84-6829aa0e8f8b" />

<img width="768" height="17" alt="Snipaste_2025-07-15_13-28-11" src="https://github.com/user-attachments/assets/3a7af969-fe02-4122-8b2a-5fc1e2ff3a53" />


<img width="620" height="29" alt="Snipaste_2025-07-15_13-29-48" src="https://github.com/user-attachments/assets/3b0b83bf-a2a0-4038-b02a-9e70fa2aff65" />
