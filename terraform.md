# Terraform


## Introduction to Infrastructure as Code

Infrastructure as code provides a more robust development and deployment process. By implementing IaC through cloud providers, pipelines can be developed and deployed in a few clicks. Cloud environments provide better tools for development, deployment, implementation, testing, maintenance, and organizing workloads while reducing costs and time.

Terraform and Ansible provide simple, readable language for writing scripts and other infrastructure code. IaC tools can be categorized as configuration management (Ansible, Puppet, Saltstack), server templating (Docker, Packer, Vagrant), and provisioning tools (Terraform, CloudFormation).

Configuration management is used to install and manage software, maintain a standard structure and version control, and be idempotent. Server templating offers pre-installed software and dependencies, virtual machines or Docker images, and immutable infrastructure. Provisioning tools deploy immutable infrastructure resources such as servers, databases, network components, etc. across multiple providers. Terraform is vendor agnostic.

Terraform can be configured to work with physical machines, VMWare, AWS, GCP, Azure, alongside hundreds of other providers. Terraform uses HCL (HashiCorp Configuration Language), a simple declarative language that provisions resources through blocks of code.

Terraform works by first initalizing the project and identifying providers, then it plans how to arrive at the desired state specified in the Terraform file, and lastly applies the plan to provision the necessary resources. Every object managed by Terraform is called a resource. Terraform records the state of the infrastructure to determine changes needed to maintain the desired state. Terraform can also import resources and manage them moving forward.

## Getting Started with Terraform

### HCL Basics

HCL consists of blocks of data that take in parameters and contain mapped keys and values. Terraform files are appended as `.tf`.

Example Terraform syntax:
`
resource "local_file" "pet" {
  filename = "/root/pets.txt"
  content = "We love pets!"
}
`

In the above example, "resource" is the block name, "local_file" is the provider_resource (i.e. Resource Type), "pet" is the resource name. Filename and content will be arguments. 

* `terraform init` checks the configuration file and initializes a working directory.
* `terraform plan` shows the plan for all the arguments that Terraform will use to apply the file.
* `terraform apply` creates the resource from the file.
* `terraform show` shows the resource details (alongside running `cat`).

When a Terraform file is updated, the old file is deleted and a new file is created, provisioning the updated resource. Run `terraform destroy` to destroy a file completely.

## Terraform Basics

### Using Terraform Providers

When initialized, Terraform downloads plugins for hundreds of infrastructure platforma via HashiCorp's plugin syntax. HashiCorp includes Official providers such as AWS, GCP, etc. as well as Verified providers such as Heroku and Digital Ocean. The remaining providers are labeled Community.

Plugins are downloaded to `/root/terraform-local-file/.terraform`. Initialization syntax generally goes `[hostname]/[org namespace]/[type]: version = _____`.

Terraform can support multiple providers in the same configuration. Each provider should be initialized as a plugin when `terraform init` is run.

### Configuration Directory

A configuration directory can contain more than one configuration (Terraform) file. It is best practice to build a `main.tf` file that builds the primary essential resource definitions. Other common files with larger directories include `variables.tf`, `outputs.tf`, and `provider.tf`.

### Using Input Variables

Hard-coded values are not best practice, it is better to use input variables that are specified in a variables file with the syntax:
`
variable "filename" {
  default = "/root/pets.txt"
}
variable "content" {
  default = We love pets!"
}
[etc.]
`

These can then be called in the configuration file with `var.[variable name]`.

### Understanding the Variable Block

Variable blocks take three parameters: `default`, `type`, and `description`. Default is the default variable value while type is the data type. Default and description are optional. 

Supported variable types include:
* string
* number
* bool
* any
* list
* set (list without duplicate values)
* map
* object
* tuple

 ### Using Variables in Terraform

It is possible to pass in variables as flags using `terraform apply -var "filename=/root/pets.txt" -var "content=We love pets!"` or as environment variables `export TF_VAR_filename="/root/pets.txt"` and `export TF_VAR_content="We love pets!"`. 

Variables can also be declared in variable definitions files with file type `.tfvars` or `.tfvars.json`.

Variable definition precedence starts at the bottom with environment variables, then `terraform.tfvars` then `*.auto.tfvars` then lastly variable flags. Variable flags override everything else.

### Resource Attributes

Use resource attribute to draw from the output of one resource as input for another. In the configuration file this can be interpolated using `${resource-type.resource-name.attribute}`.

### Resource Dependencies

Terraform creates resource in the order of the implicit detected dependencies based on called resource attributes. Resource dependencies can be explicitly specified:
`
depends_on = [
  resource-type.resource-name
]
`

## Output Variables

Output variables can be stored in an output block:
`
output pet-name {
  value = random_pet.my-pet.id
  description = "Record the pet ID"
}
`

These can be printed in the command line with `terraform output`. 

Output variables are not necessary within Terraform but can make reading configuration easier and are useful for integrating with third-party shell scripts or Ansible playbooks.


## Terraform State

### Introduction to Terraform State
