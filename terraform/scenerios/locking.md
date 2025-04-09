# **Question 2: What Happens If Two People Run the Same Terraform Script?**

### **Scenario:**
What happens if two team members run the same Terraform script with different changes?

### **Potential Issues:**
1. **Terraform State Locking (If Using Remote Backend)**:
   - If using a remote backend like **AWS S3 with DynamoDB** or **GCP Cloud Storage with a Lock Table**, Terraform locks the state file, preventing simultaneous updates.
   - The second user must wait until the first operation is complete.

2. **Conflicts When Using Local State**:
   - If both users have a local state (`terraform.tfstate`), Terraform will not detect conflicts.
   - Applying changes may overwrite each other's modifications, leading to inconsistencies.

3. **State Drift Issues**:
   - If one user changes resources and another applies before pulling the latest state, Terraform might recreate or delete resources unintentionally.

### **Best Practices to Avoid Conflicts:**
1. **Use a Remote Backend** (e.g., S3 + DynamoDB for AWS or GCS + Lock Table for GCP) to prevent simultaneous changes.
2. **Run `terraform plan` Before `apply`** to detect conflicts early.
3. **Use Branching and Review Processes** (GitOps) to manage infrastructure changes.
4. **Enable State Locking** with a backend that supports locks (e.g., DynamoDB for AWS, GCS Lock Table for GCP).
5. **Pull the Latest State** before making changes (`terraform refresh`).

By following these best practices, teams can collaborate effectively and minimize infrastructure conflicts. 🚀

