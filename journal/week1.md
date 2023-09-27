# Week 1 - Terraform & Terraform Cloud Introduction

- [Root Moudle Structure](#root-module-structure)
- [Terraform and Input Variables](#terraform-and-input-variables)
- [Configuration Drift](#configuration-drift)
- [Terraform Modules](#terraform-modules)
- [Working with Files](#working-with-files)
- [Terraform Locals & Data Sources](#terraform-locals--data-sources)
- [Working with JSON](#working-with-json)
- [Resource Lifecycle](#resource-lifecycle)
- [Terraform Data Behavior](#terraform-data-behavior)
- [Provisioners (Local-exec & Remote-exec)](#provisioners-local-exec--remote-exec)


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

## Working with Files

| Function          | Description                                                                                      | Documentation Link                                      |
|-------------------|--------------------------------------------------------------------------------------------------|---------------------------------------------------------|
| `fileexists`      | Checks the existence of a file in Terraform.                                                    | [Official Documentation](https://developer.hashicorp.com/terraform/language/functions/fileexists)                 |
| `filemd5`         | Calculates the MD5 hash of a file for integrity verification.                                     | [Official Documentation](https://developer.hashicorp.com/terraform/language/functions/filemd5)                    |
| Special `path`    | Allows referencing local paths in Terraform, including `path.module` and `path.root`.           | [Filesystem and Workspace Info](https://developer.hashicorp.com/terraform/language/expressions/references#filesystem-and-workspace-info) |

### Path Variable

- `path` is a set variable that allows us to reference local paths:
    - path.module = get the path for the current module
    - path.root = get the path for the root module
    - [Special Path Variable](https://developer.hashicorp.com/terraform/language/expressions/references#filesystem-and-workspace-info)

### Example: Copying an `index.html` file to an AWS S3 bucket:

```hcl
resource "aws_s3_object" "index_html" {
  bucket = aws_s3_bucket.website_bucket.bucket
  key    = "index.html"
  source = "${path.root}/public/index.html"
}
```

## Terraform Locals & Data Sources
- [Local Values](https://developer.hashicorp.com/terraform/language/values/locals)

```tf
locals {
  s3_origin_id = "MyS3Origin"
}
```

- [Data Sources](https://developer.hashicorp.com/terraform/language/data-sources)

```tf
data "aws_caller_identity" "current" {}
output "account_id" {
  value = data.aws_caller_identity.current.account_id
}
```

## Working with JSON
- [jsonencode](https://developer.hashicorp.com/terraform/language/functions/jsonencode)

```tf
> jsonencode({"hello"="world"})
{"hello":"world"}
```

## Resource Lifecycle
- [Meta Arguments Lifcycle](https://developer.hashicorp.com/terraform/language/meta-arguments/lifecycle)


## Terraform Data Behavior
- In Terraform, plain data values like Local Values and Input Variables lack side-effects for planning and are not valid for use in `replace_triggered_by`. To indirectly trigger replacement, leverage Terraform Data's behavior, which plans an action when inputs change.

- [Documentation](https://developer.hashicorp.com/terraform/language/resources/terraform-data).


## Provisioners (Local-exec & Remote-exec)
- [Provisioners](https://developer.hashicorp.com/terraform/language/resources/provisioners/syntax)
    - Provisioners allow you to execute commands on compute instances eg. a AWS CLI command.
    - They are not recommended because Configuration Management tools such as Ansible are a better fit


- [Local-exec](https://developer.hashicorp.com/terraform/language/resources/provisioners/local-exec)
    - This will execute command on the machine running the terraform commands eg. plan apply

```tf
resource "aws_instance" "web" {
  # ...
  provisioner "local-exec" {
    command = "echo The server's IP address is ${self.private_ip}"
  }
}
```


- [Remote-Exec](https://developer.hashicorp.com/terraform/language/resources/provisioners/remote-exec)
    - This will execute commands on a machine which you target. You will need to provide credentials such as ssh to get into the machine.

```tf
resource "aws_instance" "web" {
  # ...
  # Establishes connection to be used by all
  # generic remote provisioners (i.e. file/remote-exec)
  connection {
    type     = "ssh"
    user     = "root"
    password = var.root_password
    host     = self.public_ip
  }
  provisioner "remote-exec" {
    inline = [
      "puppet apply",
      "consul join ${aws_instance.web.private_ip}",
    ]
  }
}
```
