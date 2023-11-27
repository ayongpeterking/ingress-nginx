# NGINX Ingress Controller Installation Using Helm

This guide provides step-by-step instructions on how to install the NGINX Ingress Controller in a Kubernetes cluster using Helm.

## Prerequisites

- A running Kubernetes cluster (Kubeadm, Minikube or a cloud-based cluster like AWS, GCP, Azure).
- Helm installed on your machine. [Installation guide](https://helm.sh/docs/intro/install/).

## Installation Steps

### Step 1: Add NGINX Ingress Helm Repository

Add the NGINX Ingress repository to Helm:

```bash
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
```

### Step 2: Install NGINX Ingress Controller
Install the NGINX Ingress Controller with the release name nginx-ingress:

```bash
helm install nginx-ingress ingress-nginx/ingress-nginx
```

Customize the installation:

```bash
helm install nginx-ingress ingress-nginx/ingress-nginx --namespace my-namespace --set controller.replicaCount=2
```

### Step 3: Verify the Installation
Check if the NGINX Ingress Controller pods are running:

```bash
kubectl get pods -n <namespace>
```
Wait until the NGINX Ingress Controller pods are in the Running state.

### Step 4: Deploy a Test Application

Now, let's deploy a basic NGINX application to demonstrate how to use the NGINX Ingress Controller.

#### Deploy NGINX

Create a simple NGINX deployment:
Create a file called nginx.yaml for nginx deployment and service

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2 # tells deployment to run 2 pods matching the template
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80

---
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
```
Apply this file to create a deployment and service for NGINX

```bash
kubectl apply -f nginx.yaml
```

### Step 5: Create an Ingress Resource
Now, create an Ingress resource to expose the NGINX application.

```bash
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: nginx-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: yourdomain.com
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
Replace yourdomain.com with your domain
Save the Ingress Resource in a file a nginx-ingress.yaml and deploy it with
```bash
kubectl apply -f nginx-ingress.yaml
```
Check the Ingress Resource

```bash 
kubectl get ingress
```
```Output
NAME                 CLASS    HOSTS               ADDRESS   PORTS   AGE
nginx-ingress   nginx    kubernetes.local              80      1d
```
This setup allows the management of access to applications running on Kubernetes in a more efficient and scalable way
