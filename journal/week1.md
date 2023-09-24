# Week 1 - Terraform & Terraform Cloud Introduction

- [Root Moudle Structure](#root-module-structure)
- [Terraform and Input Variables](#terraform-and-input-variables)

## Root Module Structure

[Standard Module Structure](https://developer.hashicorp.com/terraform/language/modules/develop/structure)

```
PROJECT_ROOT
│
├── main.tf
├── variables.tf     
├── terraform.tfvars
├── providers.tf           
├── outputs.tf         
└── README.md            
```

## Terraform & Input Variables

### Variables
- Enviroment Variables
    - Set in bash terminal
- Terraform Variables
    - Set in tfvars file
- Terraform Cloud Variables
    - If sensitive they can be hidden

### Input Variables
[Terraform Input Variables](https://developer.hashicorp.com/terraform/language/values/variables)

| Flag             | Description                                | Example                                  |
|------------------|--------------------------------------------|------------------------------------------|
| `var` flag       | Sets/overrides input variables             | `terraform -var user_id="my-user_id"`   |
| `var-file` flag  | Specifies variables from an external file  | `terraform apply -var-file="myvars.tfvars"` |
| `terraform.tfvars` | Default variable file for Terraform     | N/A                                      |
| `auto.tfvars`    | Auto-loaded variable file in Terraform Cloud | N/A                                  |

