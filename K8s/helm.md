# Helm



## **Helm Project Structure**

A Helm chart consists of a predefined directory structure:
```
my-chart/
│── charts/               # Subcharts (dependencies)
│── templates/            # YAML templates for Kubernetes resources
│   ├── deployment.yaml   # Defines Deployments
│   ├── service.yaml      # Defines Services
│   ├── _helpers.tpl      # Stores reusable template functions
│── values.yaml           # Default configuration values
│── Chart.yaml            # Metadata about the chart
│── README.md             # Documentation
```


## **Common Helm Commands**

Absolutely! Here's the updated section with **how to untar a Helm chart** and **use custom values**, added to the common Helm commands. It's still clean and interview-friendly ✅

---

# 📘 Common Helm Commands (with Custom Values & Untarring)

---

### 🔧 Helm Setup & Repos

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm search repo nginx
```

---

### 📦 Download & Untar a Helm Chart

- **Download a chart**
  ```bash
  helm pull bitnami/nginx
  ```

- **Download and untar in one step**
  ```bash
  helm pull bitnami/nginx --untar
  ```

- **Create New Chart**
  ```bash
  helm create my-chart
  ```

> This creates a directory `nginx/` with all the chart files inside (templates, values.yaml, etc.).

---

### 🛠️ Use Custom Values

- **Option 1: Set inline**
  ```bash
  helm install my-nginx bitnami/nginx --set service.type=LoadBalancer
  ```

- **Option 2: Use a custom `values.yaml`**
  ```bash
  helm install my-nginx bitnami/nginx -f my-values.yaml
  ```

- **Upgrade with new values**
  ```bash
  helm upgrade my-nginx bitnami/nginx -f my-updated-values.yaml
  ```

---

### 🔁 Upgrade or Install if Not Present

```bash
helm upgrade --install my-nginx bitnami/nginx --values=my-values.yaml
```

