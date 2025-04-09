**Question:**
You have a Terraform resource that provisions a single AWS EC2 instance. How would you modify the configuration to dynamically create N instances using the same configuration? Provide an explanation of the approach.

---

**Before: Terraform Code for a Single EC2 Instance**
```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"  # Replace with a valid AMI ID
  instance_type = "t2.micro"

  tags = {
    Name = "Single-Instance"
  }
}
```

**After: Terraform Code to Provision N EC2 Instances**
```hcl
provider "aws" {
  region = "us-east-1"
}

variable "instance_count" {
  default = 3
  type    = number
}

resource "aws_instance" "example" {
  count         = var.instance_count  # Creates N instances
  ami           = "ami-0c55b159cbfafe1f0"  # Replace with a valid AMI ID
  instance_type = "t2.micro"

  tags = {
    Name = "Instance-${count.index + 1}"
  }
}
```

### **Explanation of Key Concepts:**
1. **Use of `count` Meta-Argument**:
   - `count = var.instance_count` tells Terraform to create multiple instances.
   - The value of `count` comes from a variable (`var.instance_count`).
   
2. **Variable for Scalability**:
   - The `instance_count` variable allows dynamic scaling without modifying the code.
   - To increase/decrease instances, change `instance_count` in `terraform.tfvars` or as a CLI argument.

3. **Indexing with `count.index`**:
   - Each instance gets a unique name (`Instance-1`, `Instance-2`, etc.).
   - `count.index` starts from `0`, so we add `+1` to make it human-friendly.

### **How to Use It:**
1. Initialize Terraform:
   ```sh
   terraform init
   ```
2. Plan the deployment:
   ```sh
   terraform plan -var="instance_count=5"
   ```
3. Apply the changes:
   ```sh
   terraform apply -var="instance_count=5" -auto-approve
   ```
4. Destroy the instances when no longer needed:
   ```sh
   terraform destroy -var="instance_count=5" -auto-approve
   ```

### **Alternative Approach: Using `for_each` (More Flexible)**
If you need **unique configurations per instance**, use `for_each` with a map instead of `count`:
```hcl
variable "instances" {
  type = map(string)
  default = {
    instance1 = "t2.micro"
    instance2 = "t2.small"
  }
}

resource "aws_instance" "example" {
  for_each      = var.instances
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = each.value

  tags = {
    Name = each.key
  }
}
```

### **When to Use Each Approach**
| Approach  | Use Case |
|-----------|---------|
| `count`  | Best for identical resources with variable quantity |
| `for_each` | Best when each instance has unique properties |

This allows for dynamic and scalable instance provisioning with Terraform. 🚀

