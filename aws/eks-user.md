Sure! Here's a short, step-by-step **note format** to grant **read-only access to a specific EKS namespace** using the **new Access Entry system**:

---

### 🧾 Grant Read-Only Access to a Namespace in EKS (New Access Entry Method)

1. **🔐 Create IAM Role**

   * Role trusted by `eks.amazonaws.com`
   * Used by the user or service needing access

2. **🔗 Create Access Entry**
   Use CLI:

   ```bash
   aws eks create-access-entry \
     --cluster-name <cluster-name> \
     --principal-arn arn:aws:iam::<account-id>:role/<role-name> \
     --type STANDARD
   ```

3. **👤 Find Generated Username**

   * Username will be: `eks:arn:aws:iam::<account-id>:role/<role-name>`

4. **🛡️ Create Kubernetes Role (in `dev-namespace`)**

   ```yaml
   kind: Role
   namespace: dev-namespace
   rules:
     - resources: ["pods", "services"]
       verbs: ["get", "list", "watch"]
   ```

5. **🔗 Create RoleBinding**

   ```yaml
   kind: RoleBinding
   namespace: dev-namespace
   subjects:
     - kind: User
       name: eks:arn:aws:iam::<account-id>:role/<role-name>
   roleRef:
     kind: Role
     name: read-only-role
   ```

---

✅ Done! The IAM role now has **read-only access** to just the `dev-namespace`.

Let me know if you want this as a complete YAML + CLI bundle.
