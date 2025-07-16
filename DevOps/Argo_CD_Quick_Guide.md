# 🚀 Argo CD Quick Guide

**Argo CD** is a declarative, GitOps continuous delivery tool for Kubernetes. It synchronizes your Kubernetes cluster with configuration defined in a Git repository.

---

## 📌 Key Concepts

- **App of Apps**: Deploy multiple apps by deploying one root app.
- **GitOps**: Git is the single source of truth.
- **Sync**: Argo CD reconciles what’s in the Git repo with what’s in your cluster.
- **Kustomize / Helm / Plain YAML / Jsonnet**: All supported out of the box.

---

## 🛠️ Install Argo CD

```
# Create the namespace
kubectl create namespace argocd

# Install Argo CD
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

# Expose Argo CD API server (for dev only)
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

---

## 🔐 Login to Argo CD UI

```
# Get admin password
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d
```

Access UI at: `https://localhost:8080`

Default user: `admin`

---

## 🚀 Create Your First App (CLI)

```
argocd login localhost:8080

argocd app create my-app \
  --repo https://github.com/your/repo.git \
  --path k8s/my-app \
  --dest-server https://kubernetes.default.svc \
  --dest-namespace default

argocd app sync my-app
```

---

## 🎛 Application YAML Example

```
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: my-app
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/your/repo.git
    targetRevision: HEAD
    path: k8s/my-app
  destination:
    server: https://kubernetes.default.svc
    namespace: default
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
    syncOptions:
      - CreateNamespace=true
```

---

## ✅ Sync Options

- `automated`: Argo CD will automatically sync your app.
- `prune`: Remove orphaned resources not in Git.
- `selfHeal`: Reapply if someone changes resources manually in the cluster.

---

## 🧩 App of Apps Pattern

Useful for managing multiple apps in a single place.

```
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: root-app
spec:
  source:
    path: apps
    repoURL: https://github.com/your/repo.git
  ...
```

Then inside `/apps`, define more `Application` YAMLs for each microservice.

---

## 🔧 Argo CD CLI Essentials

```
argocd app list                            # List apps
argocd app get my-app                     # Show app details
argocd app sync my-app                    # Sync the app
argocd app delete my-app                  # Delete app
argocd app logs my-app                    # Show sync logs
```

---

## 🔐 RBAC & SSO

- Argo CD supports SSO via OIDC, GitHub, GitLab, etc.
- Use `argocd-rbac-cm` ConfigMap to configure permissions.

---

## 🧠 Tips

- Use **Helm** or **Kustomize** with Argo CD to simplify manifests.
- Combine with **Argo Workflows** for GitOps pipelines.
- Use **resource hooks** to customize sync behavior.

---
