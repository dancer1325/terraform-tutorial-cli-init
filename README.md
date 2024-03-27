# Initialize a Terraform Working Directory

This is a companion repository to the ["Initialize a Terraform Working Directory"](https://developer.hashicorp.com/terraform/tutorials/cli/init) tutorial.

# Goal
* Initialize a workspace which uses
  * local modules
  * remote modules
* Check '.terraform/'
* Update
  * provider versions
  * module versions

# Prerequisites
* Terraform v1.6+

# Steps
* `terraform init`
  * output describes the steps done
  * if you change module's source or version -> 
    * `terraform init` again OR
    * `terraform get`
  * creates
    * '.terraform/'
    * '.terraform.lock.hcl'
* `terraform validate`
  * allows
    * verifying if configuration syntax is
      * valid
      * consistent
* Update requireProviders' random version & module hello's version
  * `terraform validate` throws an error
  * `terraform init` throws an error since '.terraform/modules/hello' version (right now updated) != '.terraform.lock.hcl'
  * `terraform init -upgrade`
    * upgrade to the defined in the requiredProviders &
    * both are tracked under '.terraform/providers/'
  * `terraform validate` throws an error because configuration is NOT valid for these new versions
* Adjust new model's configuration
  * `terraform validate` confirms that it's fine

# Notes
* Core Terraform workflow
  * initialize
    * == prepare the workspace 
      * == configure the backend + install all providers & modules / referred in the configuration + create a version lock file
  * plan
    * == preview the changes
  * apply
    * == make the changes defined by your plan
* '.terraform/'
  * allows
    * storing
      * providers
      * modules
  * uses
    * by Terraform each time that you `terraform validate` / `terraform plan`/ `terraform apply`
  * NOT push to the repository
* 'TF_LOG'
  * := environment variable to control the terraform logging level
    * _Examples:_ `export TF_LOG=DEBUG`, `TF_LOG=DEBUG terraform init`
* '.terraform.lock.hcl'
  * based on 'required_providers' -> records
    * versions in this run &
    * dashes in this run
  * allows
    * consistent Terraform runs in different environments
      * it will be used by other users to reproduce it
  * if versions here != terraform.tf's versions -> error thrown, forcing you to  reinitialize it
## Structure
* 'main.tf'
  * define resources & data sources
* 'terraform.tf'
  * Terraform settings
  * if you do NOT specify neither `cloud {... }` nor `backend {...}` -> by default it's local backend
* 'variables.tf'
