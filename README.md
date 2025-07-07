# 🚀 Argo CD Beginner App

This project is a simple example of how to use **Argo CD** to deploy a basic NGINX web app to a Kubernetes cluster using the GitOps approach.

---

## 🎯 What This Project Does

- Deploys a basic NGINX container in Kubernetes
- Uses Argo CD to continuously monitor this Git repo
- Syncs Kubernetes manifests from Git to the cluster

---

## 📁 Project Structure

```

argo-cd-beginner-app/
└── manifests/
├── deployment.yaml         # NGINX Deployment
├── service.yaml            # ClusterIP Service
└── kustomization.yaml      # Kustomize file for Argo CD

````

---

## ⚙️ Requirements

- A running Kubernetes cluster (Minikube, KinD, EKS, etc.)
- Argo CD installed in your cluster  
  👉 [Install Guide](https://argo-cd.readthedocs.io/en/stable/getting_started/)
- This repository cloned or forked into your GitHub account

---

## 🛠 How to Deploy with Argo CD

1. Open the Argo CD Web UI
2. Click **"New App"**
3. Fill in the following:

| Field               | Value                                                    |
|--------------------|----------------------------------------------------------|
| Application Name    | `nginx-app`                                             |
| Project             | `default`                                               |
| Repository URL      | `https://github.com/<your-username>/argo-cd-beginner-app` |
| Revision            | `HEAD`                                                  |
| Path                | `manifests`                                             |
| Cluster             | `https://kubernetes.default.svc` (default in-cluster)   |
| Namespace           | `default`                                               |
| Sync Policy         | Manual or Automatic (your choice)                       |

4. Click **Create**
5. Click **Sync** → **Synchronize**

---

## 📄 File Breakdown

### `deployment.yaml`

Deploys a single NGINX pod:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 1
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
        image: nginx:alpine
        ports:
        - containerPort: 80
````

### `service.yaml`

Exposes the NGINX pod inside the cluster:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
  - port: 80
    targetPort: 80
  type: ClusterIP
```

### `kustomization.yaml`

Allows Argo CD to process multiple manifests:

```yaml
resources:
  - deployment.yaml
  - service.yaml
```

---

## 🔍 Check the Deployment

Run the following command:

```bash
kubectl get all -n default
```

---

## 🌐 Accessing the App

For local testing:

```bash
kubectl port-forward svc/nginx-service 8080:80
```

Then open: [http://localhost:8080](http://localhost:8080)

---

## 💡 What to Try Next

* Change the NGINX image to another container
* Add an Ingress to expose it externally
* Use **auto-sync** mode in Argo CD
* Add health checks or Helm support

---

## ✅ Summary

You've just completed a full GitOps cycle using Argo CD!

* Changes in Git trigger changes in Kubernetes
* Your cluster state matches your Git repo
* You now understand the **basics of GitOps deployment**

---

📦 Feel free to fork this project and make it your own.

Happy GitOps-ing! 🔁

```

---

Let me know if you want me to generate this as a GitHub repo, add Helm support, or auto-image update features next!
```
