# Docker & Kubernetes Setup, Installation and Working

## Docker Installation

### Install Docker CLI
```bash
brew install docker
```

### Install Docker Desktop (GUI)
```bash
brew install --cask docker
```

---

## Kubernetes Installation

### Install kubectl (Kubernetes CLI)
```bash
brew install kubectl
```

or

```bash
brew install kubernetes-cli
```

---

## Check Docker and Kubernetes Versions
```bash
docker -v
kubectl version
```

---

# Building a Docker Image with a Basic HTML Site

## Static Web Page using HTML

Create a file called **index.html**

```html
<html>
<head>
<title>Docker</title>
</head>

<body>
<h1>Welcome to Docker and Kubernetes</h1>
<p>--------------------------------</p>
</body>

</html>
```

---

## Dockerfile

Create a file named **Dockerfile**

```dockerfile
FROM nginx:latest

COPY index.html /usr/share/nginx/html/index.html

EXPOSE 80
```

---

## Build Docker Image

```bash
docker build -t website:v1 .
```

---

## Run Docker Container

```bash
docker run -d -p 8080:80 website:v1
```

---

## Access the Website

Open your browser and visit:

```
http://localhost:8080
```

You should see the HTML page served by the **Nginx container**.

---

# Kubernetes Deployment

After building the Docker image, the next step is to deploy the application in Kubernetes.

---

## 1. Create Kubernetes Deployment Manifest

Create a file named **deployment.yaml**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: website-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: website
  template:
    metadata:
      labels:
        app: website
    spec:
      containers:
      - name: website
        image: website:v1
        ports:
        - containerPort: 80
```

This deployment will:

- Create **2 Pods**
- Run the Docker image **website:v1**
- Expose **container port 80**

---

## 2. Create Kubernetes Service Manifest

Create a file named **service.yaml**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: website-service
spec:
  selector:
    app: website
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
  type: NodePort
```

This service will expose the deployment inside the Kubernetes cluster.

---

## 3. Deploy the Application to Kubernetes

Apply the manifests using kubectl.

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

---

## 4. Verify the Deployment

Check if the Pods are running.

```bash
kubectl get pods
```

Check deployment status.

```bash
kubectl get deployments
```

Check the service.

```bash
kubectl get svc
```

---

## 5. Access the Application

You can access the application using **port forwarding**.

```bash
kubectl port-forward service/website-service 8080:80
```

Now open your browser and go to:

```
http://localhost:8080
```

You should see the **HTML page served from Kubernetes Pods**.

---

## 6. Verify Pods

Describe the running pod.

```bash
kubectl describe pod <pod-name>
```

View pod logs.

```bash
kubectl logs <pod-name>
```

---

# Final Architecture

```
Docker Image → Kubernetes Deployment → Kubernetes Service → Browser Access
```
