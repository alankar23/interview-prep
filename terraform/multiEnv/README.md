Sure! Here's a clean and modular Terraform project structure that provisions **separate VPCs and EC2 instances for 3 environments**: `dev`, `test`, and `prod`.

Each environment:
- Has its own VPC
- Creates a specified number of EC2 instances:
  - `dev` → 2 EC2
  - `test` → 2 EC2
  - `prod` → 4 EC2

---

## 📁 Project Structure

```
terraform-aws-multi-env/
├── main.tf
├── variables.tf
├── outputs.tf
├── environments/
│   ├── dev.tfvars
│   ├── test.tfvars
│   └── prod.tfvars
```

---

## 🧾 `main.tf`

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_vpc" "this" {
  cidr_block = var.vpc_cidr
  tags = {
    Name = "${var.env}-vpc"
  }
}

resource "aws_subnet" "this" {
  vpc_id            = aws_vpc.this.id
  cidr_block        = var.subnet_cidr
  availability_zone = "us-east-1a"
  tags = {
    Name = "${var.env}-subnet"
  }
}

resource "aws_instance" "app" {
  count         = var.instance_count
  ami           = var.ami_id
  instance_type = var.instance_type
  subnet_id     = aws_subnet.this.id

  tags = {
    Name = "${var.env}-instance-${count.index + 1}"
  }
}
```

---

## 🔧 `variables.tf`

```hcl
variable "env" {
  description = "Environment name"
  type        = string
}

variable "vpc_cidr" {
  description = "CIDR block for VPC"
  type        = string
}

variable "subnet_cidr" {
  description = "CIDR block for Subnet"
  type        = string
}

variable "instance_count" {
  description = "Number of EC2 instances"
  type        = number
}

variable "ami_id" {
  description = "AMI ID for EC2 instance"
  type        = string
  default     = "ami-0c02fb55956c7d316" # Amazon Linux 2 (adjust per region)
}

variable "instance_type" {
  description = "EC2 instance type"
  type        = string
  default     = "t2.micro"
}
```

---

## 📤 `outputs.tf`

```hcl
output "vpc_id" {
  value = aws_vpc.this.id
}

output "instance_ids" {
  value = [for instance in aws_instance.app : instance.id]
}
```

---

## 📁 `environments/dev.tfvars`

```hcl
env            = "dev"
vpc_cidr       = "10.10.0.0/16"
subnet_cidr    = "10.10.1.0/24"
instance_count = 2
```

---

## 📁 `environments/test.tfvars`

```hcl
env            = "test"
vpc_cidr       = "10.20.0.0/16"
subnet_cidr    = "10.20.1.0/24"
instance_count = 2
```

---

## 📁 `environments/prod.tfvars`

```hcl
env            = "prod"
vpc_cidr       = "10.30.0.0/16"
subnet_cidr    = "10.30.1.0/24"
instance_count = 4
```

---

## 🚀 How to Run for Each Environment

```bash
# Initialize the project
terraform init

# Plan and apply for dev
terraform plan -var-file="environments/dev.tfvars"
terraform apply -var-file="environments/dev.tfvars"

# For test
terraform apply -var-file="environments/test.tfvars"

# For prod
terraform apply -var-file="environments/prod.tfvars"
```

---

Let me know if you want:
- Separate state files per environment
- Module-based structure
- Bastion setup or load balancer
- SSH access setup

Happy Terraforming! 💻🚀