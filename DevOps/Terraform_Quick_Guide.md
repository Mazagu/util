# ⚙️ Terraform Quick Guide

Terraform is an open-source tool for provisioning and managing infrastructure as code (IaC). It enables you to define cloud and on-prem resources using a declarative configuration language called **HCL (HashiCorp Configuration Language)**.

---

## 🧱 Core Concepts

- **Providers**: Interfaces to platforms (AWS, GCP, Azure, etc.)
- **Resources**: Infrastructure elements like servers, databases, etc.
- **Modules**: Reusable containers for Terraform code
- **State**: A snapshot of your current infrastructure
- **Plan**: Preview of what Terraform will change
- **Apply**: Makes the desired changes in infrastructure
- **Destroy**: Tears down all managed resources

---

## 📁 Typical Project Structure

```
.
├── main.tf        # Main config
├── variables.tf   # Input variables
├── outputs.tf     # Output values
├── terraform.tfvars  # Default values for variables
```

---

## 🚀 Basic Workflow

```
# 1. Initialize project (download provider plugins)
terraform init

# 2. Preview changes
terraform plan

# 3. Apply changes
terraform apply

# 4. Destroy infrastructure
terraform destroy
```

---

## 🧾 Example: Provisioning an AWS EC2 Instance

```
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"

  tags = {
    Name = "MyInstance"
  }
}
```

---

## 📥 Input Variables

Define variables in `variables.tf`:

```
variable "region" {
  description = "AWS region"
  default     = "us-east-1"
}
```

Use them in config:

```
provider "aws" {
  region = var.region
}
```

Pass them with `terraform.tfvars`:

```
region = "us-west-2"
```

---

## 📤 Output Values

Define outputs in `outputs.tf`:

```
output "instance_id" {
  value = aws_instance.example.id
}
```

---

## 🗂 Remote State (Example with S3)

```
terraform {
  backend "s3" {
    bucket = "my-tf-state-bucket"
    key    = "state.tfstate"
    region = "us-east-1"
  }
}
```

---

## 🔐 Best Practices

- Use version control for all Terraform code.
- Always use `terraform plan` before `apply`.
- Store state securely (use remote state + state locking).
- Use modules for reusable infrastructure.
- Use `terraform fmt` to format code.
- Protect sensitive variables with `sensitive = true`.

---

## 🧩 Useful Commands

```
terraform validate     # Validate syntax
terraform fmt          # Format code
terraform output       # Show outputs
terraform taint        # Force resource recreation
terraform show         # Show current state
terraform graph        # Generate a dependency graph
```

---
