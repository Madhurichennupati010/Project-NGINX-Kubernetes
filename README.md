# Project-NGINX-Kubernetes


🚀 Deploy a Simple NGINX Web Application on Kubernetes using AWS EC2 & Minikube

## 📌 Project Overview

This project demonstrates how to deploy a simple **NGINX web application** on a **Kubernetes cluster** running on an **AWS EC2 Ubuntu instance** using **Minikube**.

The objective of this project is to understand the core Kubernetes components such as **Pods, Deployments, ReplicaSets, and Services**, while gaining hands-on experience with Docker, Kubernetes, and AWS.

---

## 🛠️ Technologies Used

- AWS EC2
- Ubuntu 22.04 LTS
- Docker
- Kubernetes
- Minikube
- kubectl
- NGINX

---

## 📖 Project Architecture

```
                    +--------------------+
                    |   Web Browser      |
                    +---------+----------+
                              |
                              |
                       NodePort / Port Forward
                              |
                              ▼
                 +--------------------------+
                 |      Kubernetes Service  |
                 +------------+-------------+
                              |
                              ▼
                    +------------------+
                    |   Deployment     |
                    +--------+---------+
                             |
                             ▼
                     +---------------+
                     |   NGINX Pod   |
                     +-------+-------+
                             |
                             ▼
                     +---------------+
                     | Docker Image  |
                     +---------------+
                             |
                             ▼
                 +--------------------------+
                 | Minikube Kubernetes Node |
                 +--------------------------+
                             |
                             ▼
                     AWS EC2 Ubuntu Server
```

---

# 🎯 Project Objective

Deploy an NGINX web application on Kubernetes by:

- Installing Docker
- Installing kubectl
- Installing Minikube
- Creating a Kubernetes Cluster
- Deploying an NGINX Container
- Exposing the application using a Kubernetes Service
- Accessing the application from a web browser

---

# 📋 Prerequisites

- AWS Account
- Ubuntu EC2 Instance
- Minimum EC2 Type:
  - **t3.medium**
- 20 GB EBS Volume
- Internet Connectivity

---

# 🚀 Step 1: Launch AWS EC2 Instance

Launch an Ubuntu 22.04 EC2 instance.

Recommended Configuration

- Instance Type: **t3.medium**
- Storage: **20 GB**
- Security Group Ports:

| Port | Purpose |
|------|----------|
|22|SSH|
|80|HTTP|
|8080|Port Forward|
|30000-32767|NodePort|

📷 Screenshot:

```
screenshots/ec2-instance.png
```

---

# 🚀 Step 2: Update Ubuntu

```bash
sudo apt update
sudo apt upgrade -y
```

---

# 🚀 Step 3: Install Docker

```bash
sudo apt install docker.io -y
```

Enable Docker

```bash
sudo systemctl enable docker
sudo systemctl start docker
```

Check Docker

```bash
docker --version
```

---

# 🚀 Step 4: Install kubectl

```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s \
https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
```

```bash
chmod +x kubectl
```

```bash
sudo mv kubectl /usr/local/bin/
```

Verify

```bash
kubectl version --client
```

---

# 🚀 Step 5: Install Minikube

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
```

```bash
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

Check

```bash
minikube version
```

---

# 🚀 Step 6: Start Kubernetes Cluster

```bash
minikube start --driver=docker
```

Verify

```bash
kubectl get nodes
```

Expected

```
NAME       STATUS
minikube   Ready
```

📷 Screenshot

```
screenshots/minikube.png
```

---

# 🚀 Step 7: Deploy NGINX

```bash
kubectl create deployment nginx-web --image=nginx
```

Verify

```bash
kubectl get deployments
```

```bash
kubectl get pods
```

Expected

```
Running
```

📷 Screenshot

```
screenshots/kubectl-get-pods.png
```

---

# 🚀 Step 8: Expose Deployment

```bash
kubectl expose deployment nginx-web \
--type=NodePort \
--port=80
```

Check Service

```bash
kubectl get svc
```

---

# 🚀 Step 9: Access the Application

Port Forward

```bash
kubectl port-forward service/nginx-web 8080:80 --address=0.0.0.0
```

Browser

```
http://<EC2-Public-IP>:8080
```

📷 Screenshot

```
screenshots/final-output.png
```

---

# 📂 Project Structure

```
Project-NGINX-Kubernetes/
│
├── README.md
├── deployment.yaml
├── service.yaml
├── screenshots/
│   ├── 01-ec2-instance.png
│   ├── 02-docker-installation.png
│   ├── 03-minikube-start.png
│   ├── 04-kubectl-get-nodes.png
│   ├── 05-kubectl-get-pods.png
│   ├── 06-nginx-deployment.png
│   ├── 07-nodeport-service.png
│   ├── 08-browser-output.png
│   ├── 09-memory-error.png
│   ├── 10-imagepullbackoff.png
│   ├── 11-ebs-volume-expanded.png
│   └── 12-running-pod.png
│
└── docs/
    └── Project_NGINX_Updated.docx
```

---

# 🐞 Challenges Faced

## 1️⃣ Minikube Failed to Start

### Error

```
RSRC_INSUFFICIENT_CONTAINER_MEMORY
```

### Cause

The EC2 instance type **t2.micro** provides only **1 GB RAM**, which is insufficient for running Minikube.

### Solution

- Stopped the EC2 instance.
- Changed the instance type from **t2.micro** to **t3.medium**.
- Restarted the instance.
- Successfully started Minikube.

📷 Screenshot

```
screenshots/09-memory-error.png
```

---

## 2️⃣ ImagePullBackOff

### Error

```
ImagePullBackOff
```

### Cause

Docker could not download the NGINX image because the root EBS volume was almost full.

### Solution

- Increased the EBS volume from **8 GB** to **20 GB**.
- Extended the Linux filesystem.
- Recreated the deployment.

📷 Screenshot

```
screenshots/10-imagepullbackoff.png
```

---

## 3️⃣ No Space Left on Device

### Error

```
no space left on device
```

### Solution

- Increased the EBS storage.
- Verified free space using:

```bash
df -h
```

---

# 📚 Kubernetes Commands Used

```bash
kubectl get nodes

kubectl get pods

kubectl get svc

kubectl describe pod

kubectl create deployment

kubectl expose deployment

kubectl delete deployment

kubectl logs

kubectl port-forward
```

---

# 🎯 Learning Outcomes

Through this project, I gained practical experience in:

- AWS EC2 provisioning
- Docker installation and configuration
- Kubernetes cluster setup using Minikube
- Pods
- Deployments
- ReplicaSets
- Services
- NodePort
- Port Forwarding
- Kubernetes troubleshooting
- EBS volume expansion
- Docker image management

---

# ✅ Final Result

Successfully deployed an **NGINX web application** on Kubernetes running on an **AWS EC2 Ubuntu instance** using **Docker** and **Minikube**.

The application was successfully accessed from the browser after resolving memory and storage-related issues.

---

# 👨‍💻 Author

**Madhuri Chennupati**

Aspiring AWS Cloud & DevOps Engineer

- AWS
- Docker
- Kubernetes
- Jenkins
- GitHub Actions
- Terraform
- Linux
