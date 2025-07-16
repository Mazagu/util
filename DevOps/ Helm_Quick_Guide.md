# ⛵ Helm Quick Guide

Helm is the **package manager for Kubernetes**, helping you define, install, and manage Kubernetes applications with reusable templates called **charts**.

---

## 📦 What is a Helm Chart?

A Helm chart is a collection of files that describe a set of Kubernetes resources.

```
mychart/
├── Chart.yaml        # Metadata (name, version, etc.)
├── values.yaml       # Default configuration values
├── templates/        # Kubernetes manifests (templates)
│   ├── deployment.yaml
│   ├── service.yaml
│   └── _helpers.tpl
```

---

## 🚀 Basic Helm Workflow

```
# 1. Install Helm (if not already)
brew install helm       # macOS
choco install kubernetes-helm   # Windows

# 2. Create a new chart
helm create mychart

# 3. Install a chart
helm install my-release ./mychart

# 4. Upgrade a release
helm upgrade my-release ./mychart

# 5. Uninstall a release
helm uninstall my-release

# 6. View releases
helm list
```

---

## 🛠 Template Example: `deployment.yaml`

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Release.Name }}-app
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app: {{ .Release.Name }}
  template:
    metadata:
      labels:
        app: {{ .Release.Name }}
    spec:
      containers:
        - name: app
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
          ports:
            - containerPort: 80
```

---

## 📘 Common Helm Commands

```
helm repo add bitnami https://charts.bitnami.com/bitnami
helm search repo nginx
helm fetch bitnami/nginx
helm template my-release ./mychart      # Render templates locally
helm get all my-release                 # Get release details
helm lint ./mychart                     # Lint your chart
```

---

## 🧾 values.yaml Example

```
replicaCount: 2

image:
  repository: nginx
  tag: latest

service:
  type: ClusterIP
  port: 80
```

---

## 🎛 Helm with Custom Values

```
# Override with a custom values file
helm install my-release ./mychart -f custom-values.yaml

# Override inline
helm install my-release ./mychart --set replicaCount=3
```

---

## 🧩 Useful Features

- **Dependencies**: Add charts as dependencies in `Chart.yaml`
- **Hooks**: Run lifecycle hooks for pre/post install, delete, etc.
- **Helmfile**: Manage multiple Helm charts declaratively
- **Secrets management**: Use tools like Sealed Secrets or SOPS

---

## ✅ Best Practices

- Keep `values.yaml` clean and minimal.
- Use `_helpers.tpl` for reusable template logic.
- Use `helm lint` before deploying.
- Store your charts in a chart repository or Helmfile repo.
- Use `helm diff` plugin for safer upgrades.

---
