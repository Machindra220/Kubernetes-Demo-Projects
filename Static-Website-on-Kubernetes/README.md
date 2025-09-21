# static-Website hosting on Minikube/Kind Cluster
This is just a static page for demo

---

```markdown
# 🚀 Static Webpage Hosting on Minikube

This project demonstrates how to host a static HTML webpage using **Nginx** on a local **Minikube** Kubernetes cluster. It includes a Dockerfile, Kubernetes manifests, and deployment instructions.

---

## 📁 Project Structure

```
.
├── index.html
├── css/ (Optional)
├── js/  (Optional)
├── Dockerfile
├── deployment.yaml
└── service.yaml
```

---

## 🛠️ Prerequisites

- [Minikube](https://minikube.sigs.k8s.io/docs/start/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [Docker](https://docs.docker.com/get-docker/)

---

## 🧱 Step-by-Step Setup

### 1. Clone the Repository

```bash
git clone https://github.com/Machindra220/kubernetes-demo-projects.git
cd kubernetes-demo-projects
```

### 2. Start Minikube

```bash
minikube start
```
## 📦 Dockerfile Overview

```Dockerfile
FROM nginx:alpine
COPY . /usr/share/nginx/html
```

### 3. Build Docker Image

```bash
docker build -t static-page:v1 .
```

### 4. Load Image into Minikube

```bash
minikube image load static-page:v1
```

---

## 📄 Kubernetes Manifests

### `deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: static-page
spec:
  replicas: 1
  selector:
    matchLabels:
      app: static-page
  template:
    metadata:
      labels:
        app: static-page
    spec:
      containers:
      - name: nginx
        image: static-page:v1
        ports:
        - containerPort: 80
```

### `service.yaml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: static-page-service
spec:
  type: NodePort
  selector:
    app: static-page
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
    nodePort: 30080
```


### 5. Deploy to Kubernetes

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

### 6. Access the Webpage

#### Option A: Port Forwarding

```bash
kubectl port-forward svc/static-page-service 8080:80
```

Visit: `http://localhost:8080`

#### Option B: Minikube Service Helper

```bash
minikube service static-page-service
```

---


---

## 🧠 Notes

- You can customize the HTML/CSS/JS files to suit your project
- For production, consider using Ingress and TLS
- Works great with Rancher for visual management

---

## 📜 License

MIT License © Machindra220
