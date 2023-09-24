# Week 0 - Prep Work

- [Semantic Versioning](#semantic-versioning)
- [Install the Terraform CLI](#terraform-cli)
- [Environment Variables](#environment-variables)
- [Terraform Basics](#terraform-basics)


## Semantic Versioning 

This project is going utilize semantic versioning for its tagging.
[semver.org](https://semver.org/)

The general format:

 **MAJOR.MINOR.PATCH**, eg. `1.0.1`

- **MAJOR** version when you make incompatible API changes
- **MINOR** version when you add functionality in a backward compatible manner
- **PATCH** version when you make backward compatible bug fixes

## Terraform CLI

- To improve DevX, an [install script](./bin/install_terraform_cli) was created to install the CLI. The [official documentation](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli) was used as a reference. The script is called during the **before** [life-cycle-state](https://www.gitpod.io/docs/configure/workspaces/workspace-lifecycle) in [gitpod.yml](./.gitpod.yml) in the GitPod development environment.

## Environment Variables

- Project root location stored in **$PROJECT_ROOT**
- Use `gp env` to set persisting env vars in GitPod Workspaces
    - Can also be done in [gitpod.yml](./.gitpod.yml) for non-sensitive variables
- Use `.bash_profile` files set persisting env vars across bash terminals
- [.env.example](./.env.example) will act as a template for all env vars needed for this project

## AWS CLI

- AWS CLI install script [`./bin/install_aws_cli`](./bin/install_aws_cli)
    - [Getting Started Install (AWS CLI)](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
    - [AWS CLI Env Vars](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html)

- Check AWS Creds with:
    ```sh
    aws sts get-caller-identity
    ```
- If done right, the following should be returned:
    ```json
    {
        "UserId": "...",
        "Account": "123456789012",
        "Arn": "arn:aws:iam::123456789012:user/terraform-beginner-bootcamp"
    }
    ```

## Terraform Basics

### Terraform Registry

- [Registry](https://registry.terraform.io/)
- **Providers**: Interface to APIs
- **Modules**: Add modular logic to create more portable and sharable IaC

### Terraform CLI

Important Commands
- `terraform init`
- `terraform plan`
- `terraform apply`
- `terraform apply --auto-approve`
- `terraform destroy`

### Terraform Lock & State Files
- `.terraform.lock.hcl` contains the locked versioning for the providers or modulues
    - **Should be comitted**
- `.terraform.tfstate` contains information about the current state of your infrastructure
    - **Should not be commited**
    - May contain sensitive data

### Terraform Directory
- `.terraform` directory contains binaries of terraform providers.


