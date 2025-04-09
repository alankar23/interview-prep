# **Scenario-Based Terraform Questions & Answers**

## **1. How do you manage Terraform state in a team environment to avoid conflicts?**

### **Solution:**
- Use a **remote backend** (e.g., AWS S3 + DynamoDB, GCP Cloud Storage + Lock Table, Terraform Cloud).
- Enable **state locking** to prevent simultaneous updates.
- Use `terraform plan` before `apply` to detect potential issues.
- Implement **Git workflows** (e.g., feature branches, PR approvals) for controlled changes.

---

## **2. How would you roll back infrastructure changes if an apply goes wrong?**

### **Solution:**
- Use **Terraform versioning** to track state history (remote backends store versions).
- Run `terraform state pull` to review the current state before making corrections.
- If using Terraform Cloud, utilize the **"State Rollback"** feature.
- Manually edit the `.tf` files, then apply again to revert changes.
- In severe cases, use `terraform destroy` and redeploy from the last known good configuration.

---

## **3. How do you handle secrets in Terraform without exposing them in the code?**

### **Solution:**
- Use **Terraform Vault Provider** (e.g., HashiCorp Vault, AWS Secrets Manager, GCP Secret Manager).
- Store secrets in environment variables and use `terraform.tfvars` (add `terraform.tfvars` to `.gitignore`).
- Use `sensitive` variables to mask output: 
  ```hcl
  variable "db_password" {
    type      = string
    sensitive = true
  }
  ```
- Use remote state encryption (e.g., **S3 server-side encryption**).

---

## **4. What happens if an existing resource is modified outside of Terraform?**

### **Solution:**
- Run `terraform plan` to detect **drift** (differences between the actual state and Terraform state).
- Use `terraform apply` to bring the resource back to the desired configuration.
- Use `terraform import` if you want to retain the manual changes.
- Enable **CloudTrail (AWS)** or **Audit Logging (GCP/Azure)** to track unexpected changes.

---

## **5. How would you use Terraform to deploy an application across multiple environments (dev, staging, prod)?**

### **Solution:**
- Use **workspaces** (`terraform workspace new dev`) to manage different environments.
- Use separate state files and backend configurations for each environment.
- Structure directories like this:
  ```
  ├── environments/
  │   ├── dev/
  │   │   ├── main.tf
  │   │   ├── variables.tf
  │   │   ├── backend.tf
  │   ├── staging/
  │   ├── prod/
  ```
- Use `terraform apply -var-file=dev.tfvars` for environment-specific configurations.

---

## **6. How can you ensure zero downtime deployment using Terraform?**

### **Solution:**
- Use **rolling updates** with `create_before_destroy` in Terraform:
  ```hcl
  resource "aws_launch_configuration" "example" {
    lifecycle {
      create_before_destroy = true
    }
  }
  ```
- Deploy resources in **phases** (e.g., use Blue-Green or Canary deployments).
- Use **load balancers** to manage traffic between old and new infrastructure.
- Use **Terraform Modules** for infrastructure versioning and quick rollback.

---

## **7. How do you handle module versioning in Terraform?**

### **Solution:**
- Define version constraints in `module` blocks:
  ```hcl
  module "network" {
    source  = "git::https://github.com/example/network.git//module_path"
    version = "~> 1.0"
  }
  ```
- Store module versions in **Terraform Registry** or **Git tags/branches**.
- Use `terraform version` to check compatibility before applying changes.

---

## **8. What is the difference between `terraform taint` and `terraform destroy` followed by `terraform apply`?**

### **Solution:**
- `terraform taint`: Marks a resource for **re-creation** on the next `apply` without affecting dependencies.
- `terraform destroy && terraform apply`: Completely **destroys and recreates** the resource, which may lead to downtime and dependency issues.
- `terraform taint` is preferred for rolling updates without full deletion.

---

## **9. How do you dynamically provision infrastructure in multiple regions using Terraform?**

### **Solution:**
- Use **count** or **for_each** with region maps:
  ```hcl
  variable "regions" {
    type = list(string)
    default = ["us-east-1", "us-west-2"]
  }

  resource "aws_s3_bucket" "example" {
    for_each = toset(var.regions)
    bucket   = "my-bucket-${each.value}"
    provider = aws.${each.value}
  }
  ```
- Use **provider aliases** to configure different regions dynamically.
- Use Terraform Cloud workspaces for multi-region deployments.

---

## **10. How would you enforce security policies using Terraform?**

### **Solution:**
- Use **Terraform Sentinel** or **OPA (Open Policy Agent)** for policy enforcement.
- Define IAM policies using Terraform:
  ```hcl
  resource "aws_iam_policy" "example" {
    policy = jsonencode({
      "Version": "2012-10-17",
      "Statement": [
        {
          "Effect": "Deny",
          "Action": "s3:*",
          "Resource": "*"
        }
      ]
    })
  }
  ```
- Use **GitOps & CI/CD Pipelines** to enforce security scanning before applying infrastructure changes.
- Implement **network security rules** using Terraform security groups.

---

## **Conclusion:**
By implementing best practices and understanding these scenarios, you can manage Terraform infrastructure efficiently and securely in real-world environments. 🚀

