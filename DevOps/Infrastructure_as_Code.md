# 🛠️ Quick Guide: Infrastructure as Code (IaC)

Infrastructure as Code (IaC) is the practice of managing and provisioning infrastructure using machine-readable configuration files, rather than manual processes.

---

## ✅ Benefits of IaC

- **Consistency**: Avoid configuration drift between environments.
- **Speed**: Provision infrastructure quickly and repeatedly.
- **Version Control**: Store and audit infrastructure changes via Git.
- **Automation**: Integrate with CI/CD pipelines for full automation.
- **Scalability**: Easily replicate infrastructure for different environments.

---

## 🧰 Common IaC Tools

| Tool        | Language   | Best For                            |
|-------------|------------|--------------------------------------|
| Terraform   | HCL        | Cloud-agnostic, modular infrastructure |
| Pulumi      | TypeScript, Python, Go, etc. | IaC with familiar programming languages |
| AWS CloudFormation | YAML/JSON | AWS-native infrastructure |
| Ansible     | YAML       | Configuration management & provisioning |
| Chef/Puppet | Ruby DSL   | Configuration enforcement at scale |

---

## 🔧 Example: Terraform

```
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "web" {
  ami           = "ami-0abcdef1234567890"
  instance_type = "t2.micro"
}
```

- **Declare** what infrastructure you want
- **Plan** the changes (`terraform plan`)
- **Apply** the changes (`terraform apply`)

---

## 📦 Declarative vs Imperative

- **Declarative**: You define *what* the desired state is (e.g., Terraform, CloudFormation).
- **Imperative**: You define *how* to reach the desired state step-by-step (e.g., Bash scripts).

---

## 🛡️ Best Practices

- Keep IaC files in **version control**
- Use **remote backends** for storing state securely (e.g., S3 for Terraform)
- Apply **modularization** (use reusable modules)
- Separate **environments** (dev/staging/prod) clearly
- Always run **`plan` before `apply`**
- Use **secret managers** (e.g., Vault, AWS Secrets Manager) instead of hardcoding credentials

---

## 🔄 Integrate with CI/CD

- Run IaC in **CI pipelines** to validate syntax and enforce policies (e.g., `terraform validate`)
- Trigger deployments with **pull requests** or tagged commits
- Use **`terraform destroy`** with caution (and never on production automatically)

---

## 📚 Related Concepts

- **Immutable Infrastructure**: Rebuild servers from scratch instead of changing them
- **GitOps**: Git is the source of truth for deployments
- **Policy as Code**: Enforce governance using tools like OPA (Open Policy Agent)

---

Infrastructure as Code is a cornerstone of modern DevOps — enabling teams to move fast, stay consistent, and reduce errors.
