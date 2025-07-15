# **Minikube Setup and Kubernetes Orchestration**  

## **Project Overview**  
This project guides you through setting up **Minikube**, a lightweight Kubernetes implementation, to create a local Kubernetes cluster for development and testing. By the end of this project, you will:  
- Understand **Kubernetes** and its core components.  
- Learn how to **deploy Minikube** on Windows, Linux, or macOS.  
- Gain hands-on experience with **Docker** and **Kubernetes CLI (kubectl)**.  
- Build and deploy applications on a **local Kubernetes cluster**.  

---

## **Table of Contents**  
1. [Introduction to Kubernetes](#introduction-to-kubernetes)  
2. [Kubernetes Architecture](#kubernetes-architecture)  
3. [Project Prerequisites](#project-prerequisites)  
4. [Installation Guide](#installation-guide)  
   - [Windows](#windows-installation)  
   - [Linux](#linux-installation)  
   - [macOS](#macos-installation)  
5. [Starting Minikube](#starting-minikube)  
6. [Verifying the Setup](#verifying-the-setup)  
7. [Basic Kubernetes Operations](#basic-kubernetes-operations)  
8. [Troubleshooting](#troubleshooting)  
9. [Conclusion](#conclusion)  

---

## **Introduction to Kubernetes**  
Kubernetes (K8s) is an **open-source container orchestration platform** that automates the deployment, scaling, and management of containerized applications. Originally developed by Google, Kubernetes is now the **de facto standard** for managing distributed applications in production.  

### **Key Features of Kubernetes**  
✔ **Automated Scaling** – Dynamically adjusts workloads based on demand.  
✔ **Self-Healing** – Restarts failed containers and replaces unhealthy nodes.  
✔ **Load Balancing** – Distributes network traffic efficiently.  
✔ **Rolling Updates** – Deploys updates without downtime.  

---

## **Kubernetes Architecture**  
A Kubernetes cluster consists of:  

### **1. Control Plane (Master Node)**  
- **API Server** – Entry point for cluster interactions.  
- **Scheduler** – Assigns workloads to nodes.  
- **Controller Manager** – Ensures the cluster state matches the desired state.  
- **etcd** – Distributed key-value store for cluster data.  

### **2. Worker Nodes**  
- **Kubelet** – Manages containers on the node.  
- **Kube Proxy** – Handles network routing.  
- **Container Runtime (Docker, containerd)** – Runs the containers.  

### **3. Pods & Containers**  
- **Pods** – Smallest deployable units (one or more containers).  
- **Containers** – Lightweight, portable application environments.  

---

## **Project Prerequisites**  
Before starting, ensure your system meets these requirements:  

✅ **2 CPUs or more**  
✅ **2GB of free memory**  
✅ **20GB of free disk space**  
✅ **Docker installed** (for container runtime)  

---

## **Installation Guide**  

### **Windows Installation**  
1. **Install Chocolatey (Windows Package Manager)**  
   ```powershell
   Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
   ```
2. **Install Minikube**  
   ```powershell
   choco install minikube
   ```
3. **Install Docker Desktop**  
   Download from [Docker Desktop for Windows](https://www.docker.com/products/docker-desktop).  

---

### **Linux Installation**  
1. **Install Docker**  
   ```bash
   sudo apt-get update
   sudo apt-get install docker.io
   sudo systemctl enable --now docker
   ```
2. **Install Minikube**  
   ```bash
   curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
   sudo install minikube-linux-amd64 /usr/local/bin/minikube
   ```
3. **Install kubectl**  
   ```bash
   sudo snap install kubectl --classic
   ```

---

### **macOS Installation**  
1. **Install Homebrew (if not installed)**  
   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```
2. **Install Minikube**  
   ```bash
   brew install minikube
   ```
3. **Install Docker Desktop**  
   Download from [Docker Desktop for Mac](https://www.docker.com/products/docker-desktop).  

---

## **Starting Minikube**  
Run the following command to start Minikube with Docker as the driver:  

```bash
minikube start --driver=docker
```
Verify the cluster is running:  
```bash
kubectl get nodes
```
Expected output:  
```
NAME       STATUS   ROLES    AGE   VERSION
minikube   Ready    master   1m    v1.27.3
```

---

## **Verifying the Setup**  
1. **Check Minikube Status**  
   ```bash
   minikube status
   ```
2. **Access Kubernetes Dashboard**  
   ```bash
   minikube dashboard
   ```
3. **Deploy a Test Application**  
   ```bash
   kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.4
   kubectl expose deployment hello-minikube --type=NodePort --port=8080
   minikube service hello-minikube
   ```

---

## **Basic Kubernetes Operations**  
| Command | Description |  
|---------|-------------|  
| `kubectl get pods` | List all running pods |  
| `kubectl apply -f deployment.yaml` | Deploy using a YAML file |  
| `kubectl delete pod <pod-name>` | Delete a pod |  
| `kubectl logs <pod-name>` | View pod logs |  
| `kubectl describe pod <pod-name>` | Get detailed pod info |  

---

## **Troubleshooting**  
❌ **Minikube fails to start** → Ensure Docker is running.  
❌ **"kubectl not found"** → Reinstall kubectl.  
❌ **Insufficient memory** → Allocate more resources:  
   ```bash
   minikube start --driver=docker --memory=4096 --cpus=2
   ```

---

