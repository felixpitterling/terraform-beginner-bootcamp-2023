# Week 1 - Terraform & Terraform Cloud Introduction

- [Root Moudle Structure](#root-module-structure)
- [Terraform and Input Variables](#terraform-and-input-variables)
- [Configuration Drift](#configuration-drift)
- [Terraform Modules](#terraform-modules)


#### **[Journal Overview ←](./../README.md#weekly-journals)**

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


## Configuration Drift

### What happens if we lose our state file?
- You will have to destroy your cloud infrastructure manually.
- You can use terraform port for some resources

### Fix Missing Resources with Terraform Import
- `terraform import aws_s3_bucket.bucket bucket-name`
- [Terraform Import](https://developer.hashicorp.com/terraform/cli/import)
- [AWS S3 Bucket Import](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/s3_bucket#import)

### Fix using Terraform Refresh
```sh
terraform apply -refresh-only -auto-approve
```


## Terraform Modules
- It is recommend to place modules in a `./modules` directory

### Passing Input Variables
- Example:
    ```tf
    module "terrahouse_aws" {
    source = "./modules/terrahouse_aws"
    user_uuid = var.user_uuid
    bucket_name = var.bucket_name
    }
    ```

### Modules Sources
- [Modules Sources](https://developer.hashicorp.com/terraform/language/modules/sources)
    - Local
    - Github
    - Terraform Registry

- Example: 
    ```tf
    module "terrahouse_aws" {
    source = "./modules/terrahouse_aws"
    }
    ```