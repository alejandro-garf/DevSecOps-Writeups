# TryHackMe Room Writeup — Cloud-Based IaC (Terraform & CloudFormation)

---

## Terraform 101

Terraform is an IaC tool used for provisioning that allows users to define both cloud and on-prem resources in a human-readable configuration file that can be versioned, reused, and distributed across teams.

### Core Components

| Component | Description |
|---|---|
| **Terraform Core** | Responsible for the core functionalities that allow users to provision and manage infrastructure. |
| **State** | Keeps track of what the infrastructure currently looks like. |
| **Config Files** | Defines what you want the infrastructure to look like. |
| **Provider** | What will be actioned to make the required changes to reach the desired state. |

---

## Terraform Configuration

- Terraform config files are written in a declarative language called **HCL (HashiCorp Configuration Language)**.
- HCL allows for the **modularisation** of infrastructure.
- The **provider** block is defined before the resource block.
- **`main.tf`** acts as the main configuration file.
- **`variables.tf`** can be used instead of repeatedly defining values across multiple infrastructure modules.

---

## Terraform Workflow

The Terraform workflow follows four steps: **Write → Initialise → Plan → Apply**.

| Command | Description |
|---|---|
| `terraform init` | Initialises the infrastructure. |
| `terraform plan` | Plans and previews your changes before applying. |
| `terraform apply` | Applies changes to reach the desired infrastructure state. |

> **Day N** denotes a day in the future when the infrastructure is no longer needed and can be destroyed.

---

## CloudFormation 101

**CloudFormation** is an AWS IaC tool for automated provisioning and resource management.

Key features:
- **Events** are generated during stack creation.
- **Rollback triggers** can be defined in a template.
- **Cross-stack references** allow you to refer to resources in another stack.

---

## CloudFormation Configuration and Use Cases

- CloudFormation supports **intrinsic functions** within templates.
- **Change sets** help you understand the impact of modifications before they are applied.

---

## Terraform vs CloudFormation

| Feature | Terraform | CloudFormation |
|---|---|---|
| Cloud agnostic | ✅ Yes | ❌ No — AWS only |
| Native AWS integration | Limited | ✅ Deep integration |
| Community modules | ✅ Large community-driven module library | Limited |

---

## Secure IaC

Best practices for securing IaC:

- **Parameterise sensitive data** instead of hardcoding it into configuration files.
- **Code reviews** can catch and patch security issues early.
- **Stack policies** are used in CloudFormation for update controls, preventing unintended changes to critical resources.

---

## Practical

Follow through the site exercise to complete the task.

> Flag: `THM{c10uD-b@z3d-1@SeE}`
