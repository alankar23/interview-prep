## Connect to S3
---

### 🧾 IRSA - IAM Roles for Service Accounts (EKS)

* **Purpose**: Securely grant AWS permissions to Kubernetes pods.
* **Use case**: When a pod needs to call AWS services (e.g., S3, DynamoDB).
* **Key Concept**: Link a Kubernetes service account to an IAM role using EKS OIDC.

---

### 🔧 How it Works

1. **Enable OIDC Provider** on EKS:

   * Needed for federated identity via web tokens.

2. **Create IAM Role** with:

   * Trust policy allowing EKS OIDC + service account.

3. **Create ServiceAccount** in Kubernetes:

   * Annotate it with `eks.amazonaws.com/role-arn`.

4. **Deploy Pod** using that ServiceAccount:

   * Pod inherits IAM permissions from the linked role.

---

### ✅ Benefits

* Fine-grained IAM permissions at pod level.
* Avoids sharing long-term credentials.
* More secure than using node IAM roles.

---

### 🔐 Example Use Cases

* S3 access from pods.
* DynamoDB access from applications.
* CloudWatch logging agents.

