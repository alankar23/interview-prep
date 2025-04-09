Great question — if you want to **create, inspect, or manage a particular resource** in your Terraform project **without affecting others**, there are a few handy commands and patterns you can use.

---

## 🔍 **1. Inspect a Particular Resource**

### 👉 `terraform state show <resource_address>`

**Example:**
```bash
terraform state show aws_instance.app[0]
```

> This shows the actual state (attributes) of that resource stored in Terraform state.

---

## 🧪 **2. Plan for a Specific Resource**

### 👉 `terraform plan -target=<resource_address>`

**Example:**
```bash
terraform plan -target=aws_vpc.this
```

> This plans **only for the VPC**, so you can safely preview changes before affecting the full project.

---

## ⚒️ **3. Apply a Specific Resource Only**

### 👉 `terraform apply -target=<resource_address>`

**Example:**
```bash
terraform apply -target=aws_instance.app[0]
```

> This will create/apply changes **only for the specified resource**.

---

## 📦 **4. Create Only One Resource Initially (e.g., EC2)**

Instead of applying the full infra, run:
```bash
terraform apply -target=aws_instance.app
```
This helps when you want to:
- Spin up EC2 before provisioning others
- Test a particular change safely

---

## 💡 Pro Tip: Find Resource Addresses

You can find all resource addresses by running:
```bash
terraform state list
```

This will output:
```
aws_vpc.this
aws_subnet.this
aws_instance.app[0]
aws_instance.app[1]
...
```

---

## 🧼 Bonus: Remove a Resource from State (No Deletion)

If you want Terraform to forget a resource (but NOT destroy it):

```bash
terraform state rm <resource_address>
```

---

Let me know if you want help targeting a resource with **count**, **for_each**, or **modules** — they behave a bit differently!