
# Argo CD Demo on GCP

This demo walks through setting up a Kubernetes cluster on **Google Cloud Platform (GCP)** and installing **Argo CD** for GitOps-based deployments.

---

## Commands Used in the Demo

### 1. Authenticate and Set Project
```bash
gcloud auth login
gcloud config set project <PROJECT_ID>
````

### 2. Enable Required Services

```bash
gcloud services enable container.googleapis.com
```

### 3. Create GKE Cluster

```bash
gcloud container clusters create argocd-demo --zone us-central1-a --num-nodes 2
```

### 4. Get Cluster Credentials

```bash
gcloud container clusters get-credentials argocd-demo --zone us-central1-a
```

### 5. Verify Nodes

```bash
kubectl get nodes
```

### 6. Install Argo CD

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

### 7. Check Argo CD Pods

```bash
kubectl get pods -n argocd
```

### 8. Expose Argo CD Server

* **Windows**

```bash
kubectl patch svc argocd-server -n argocd -p "{\"spec\":{\"type\":\"LoadBalancer\"}}"
```

* **Linux / macOS**

```bash
kubectl patch svc argocd-server -n argocd -p '{"spec":{"type":"LoadBalancer"}}'
```

### 9. Get Argo CD Service

```bash
kubectl get svc argocd-server -n argocd
```

### 10. Get Initial Admin Password

```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

### 11. Verify Deployed App (Example: nginx-demo)

```bash
kubectl get deployment nginx-demo -n default -o=jsonpath='{.spec.template.spec.containers[*].image}'
```

---

## Notes

* Replace `<PROJECT_ID>` with your actual Google Cloud project ID.
* Use the appropriate `kubectl patch` command depending on your OS.
* The initial admin username is `admin`.
