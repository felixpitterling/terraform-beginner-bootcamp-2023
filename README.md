# Terraform Beginner Bootcamp 2023

- [Semantic Versioning](#semantic-versioning)
- [Install the Terraform CLI](#terraform-cli)
- [Environment Variables](#environment-variables)


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
