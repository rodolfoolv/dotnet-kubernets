Simple **ASP.NET Core Web API** project designed to run locally using **Minikube** and **Kubernetes**.

---

## 📋 Prerequisites

- [.NET 8 SDK](https://dotnet.microsoft.com/download)
- [Docker](https://www.docker.com/products/docker-desktop)
- [Kubectl](https://kubernetes.io/docs/tasks/tools/)
- [Minikube](https://minikube.sigs.k8s.io/docs/start/)

---

## 🚀 How to Run Locally

### 1. Build the Project

```bash
dotnet build
```

### 2. Run the API Locally

```bash
dotnet run --project ApiKubernets
```

The API will be available at:

```
http://localhost:5000/weatherforecast
```

---

## 🐳 Build the Docker Image for Minikube

First, configure your terminal to use Minikube's Docker daemon:

```bash
eval $(minikube docker-env)
```

Then, build the Docker image:

```bash
docker build -t apikubernets:latest .
```

---

## ☸️ Deploy to Kubernetes (Minikube)

### 1. Apply the Kubernetes Manifests

```bash
kubectl apply -f deployment.yaml
```

This will create:

- A **Deployment** running the API container
- A **Service** of type **NodePort** exposing the application on port `30001`

### 2. Check Pods and Services

```bash
kubectl get pods
kubectl get services
```

Make sure the `apikubernets-deployment` pod status is **Running**.

---

## 🌐 Access the API

1. Get Minikube's IP:

```bash
minikube ip
```

2. Access your API using the IP and NodePort:

```
http://<MINIKUBE-IP>:30001/weatherforecast
```

Example:

```
http://192.168.49.2:30001/weatherforecast
```

You can test it using a browser, [Insomnia](https://insomnia.rest/), or [Postman](https://www.postman.com/).

---

## 📄 Kubernetes `deployment.yaml` Example

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: apikubernets-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: apikubernets
  template:
    metadata:
      labels:
        app: apikubernets
    spec:
      containers:
      - name: apikubernets
        image: apikubernets:latest
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: apikubernets-service
spec:
  type: NodePort
  selector:
    app: apikubernets
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 30001
```

---

## 🛠️ Useful Commands

- Start a tunnel (optional if NodePort doesn’t work properly):

```bash
minikube tunnel
```

- See pod logs:

```bash
kubectl logs <pod-name>
```

- Delete all resources:

```bash
kubectl delete all --all
```

---

## 📜 License

This project is licensed under the MIT License.
