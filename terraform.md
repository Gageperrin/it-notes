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

When `terraform apply` is run, a `terraform.tfstate` file is generated that provides the built-in infrastructure for Terraform. This takes truth priority over regular `.tf` files. In effect, a state file is a blueprint for real-world infrastructure. The state file also tracks metadata dependencies. If resources are deleted from a configuration file, Terraform uses state metadata to track deleted configuration resources and will delete the corresponding resources accordingly. 

In larger infrastructures, Terraform uses caching to save performance and use the meta-data as a reference of truth. With developer teams, it is important to ensure no one else is working with Terraform at the same time and that the latest configuration file is used. It is best to backup the `.tfstate` file in a remote directory.

The `.tfstate` file contains senstivie information and should not be stored or distributed without security measures. Remote State Backends should be stored in private repositories such as Amazon S3 or Terraform Cloud while version control can be uploaded to GitHub or BitBucket.

Do not manually edit the `terraform.tfstate` file manually but use commands instead.

## Working with Terraform

### Terraform Commands

* `terraform validate` can be used to check if a configuration is valid with specific information if there is a problem.
* `terraform fmt` formats local configuration files into a more readable, standardized format.
* `terraform providers` to show provider plugins required by the present configurations.
* `terraform output` shows output variables.
* `terraform refresh` syncs state files and resources.
* `terraform graph` generates a graph of configuration data that can be viewed through software such as `graphviz`.

### Mutable vs Immutable Infrastructure

Mutable infrastructures can be modified or updated while accounting for their various dependencies. Within a pool, servers can develop different software or OS versions. This is called configuration drift.


### Lifecycle Rules

By default, an updated resource is deleted before the new one is provisioned. This can be changed with lifecyle rules:

`
lifecycle  {
  create_before_destroy - true
}
`

Other options include `prevent_destroy` for databases (this **does not** override `terraform destroy`), `ignore_changes` to prevent `terraform apply` from reverting tags or other changes in a configuration file that conflict with a server, among other lifecycle rules.

Immutable infrastructure cannot be updated without provisioning new infrastructure. This allows for more stringent oversight of infrastructure maintenance and prevents configuration drift.

### Datasources

Datasources allow Terraform to read attributes of resources provisioned outside its control. A data block can be specified in the configuration file with syntax:
`
data "local_file" "dog" {
  filename = "root/dog.txt"
}
`

It is written like a resource block but with data specified instead. A data source can only read infrastructure and cannot be created, update, or destroyed like other Terraform resources. 

Data can be then be called through `data.local_file.filename`.

### Meta-Arguments

Meta-arguments can be used to replicate commands in the same way as shell scripting is used. Previous examples have been `depends_on` and `lifecycle`.

`count` is another popular meta-argument. If `count = 3` is specified within a resource block, three resources will be generated. However, these will be identical and is not a best practice. It is better to specify a list of variable defaults for Terraform to cycle through and specify these in the main configuration file. For example, `filename = var.filename[count.index]`. For an abstracted count of variables, use `count = length(var.filename)` for example to generate a count based on how many items are in the variable array.

`for_each` is a better argument to use instead of `count` for navigating maps or sets. However it cannot sort through strings. However a list of strings can be converted to a set mid-process using `for_each = toset(var.filename)`.

### Version Constraints

Inconsisent versioning can cause problems for maintaining configurations. It is possible to use earlier versions of a plugin provider using `terraform` code blocks with `source` and `version` arguments within the object. The `version` argument can also take comparative operators to specify certain version ranges including pessimistic constraint operator `~`.

## Terraform with AWS

(I skipped several of these lessons as I am already proficient in many of these conceptual and technical aspects of AWS.)

### IAM with Terraform

To create an IAM user with Terraform, create a resource block like so:
`
resource "aws_iam_user" "admin-user" {
  name = "kevin"
  tags = {
    Description = "IT Director"
  }
}
`
Even though IAM resources are global, Terraform expects a region to be specified when provisioning an IAM resource. This can be done through:
`
provider "aws" {
  region = "us-east-1"
}
`

IAM policies can be provisioned through Terraform with a resource like:
`
resource "aws_iam_policy" "adminUser" {
  name = "AdminUsers"
  policy = <<EOF
  [JSON document]
  EOF
}
`

`EOF` (end of file) can import a JSON document that reads in the IAM policy. This needs to be implemented alongside the `<<DELIMITER` and `DELIMITER` commands (Heredoc syntax). This can also be done through `policy = file("file-name.json")`.

To attach a policy create a resource of type `"aws_iam_user_policy_attachment"` with the `user` and `policy_arn` as arguments.


### S3 with Terraform

Example S3 bucket Terraform syntax:
`
resource "aws_s3_bucket" "finance" {
  bucket = "finance-17012021"
  tags = {
    Description "Payroll"
  }
}

resource "aws_s3_bucket_object" "finance-2020" {
  content = "/root/finance/finance-2020.doc"
  key = "finance-2020.doc"
  bucket = aws_s3_bucket.finance.id
}

data "aws_iam_group" "finance-data" {
  group_name = "finance-analysts"
}

resource "aws_s3_bucket_policy" "finance-policy" {
  bucket = aws_s3_bucket.finance.id
  policy = <<EOF
  [JSON file]
  EOF
}
`

The first resource provisions a bucket, the second resource uploads a file to the bucket, the third creates a IAM group, and the fourth assigns a bucket policy. (Inside the bucket policy the IAM group can be assigned.)

### DynamoDB with Terraform

Example of DynamoDB provisioning with Terraform:
`
resource "aws_dynamodb_table" "cars" {
  name = "cars"
  hash_key = "VIN"
  billing_mode = "PAY_PER_REQUEST"
  attribute {
    name = "VIN"
    type = "S"
  }
}

resource "aws_dynamodb_table_item" "car-items" {
  table_name = aws_dynamodb_table.cars.name
  hash_key = aws_dynamodb_table.cars.hash_key
  item = <<EOF
  
  {
    "Manufacturer: {"S": "Toyota"},
    "Make": {"S": "Corolla"},
    "Model": {"N": "2001"},
    "VIN" : {"S": "XXXXXXXX"},
  }
EOF
}
`

## Remote State

State files map configruations to real world resources, it tracks meta-data, improves performance, and enables collaboration. However, it does not need to only be stored locally but can be implemented remotely. Remote state backends are best practice to protect sensitive data and better enforce version control.

With local state files, Terraform can lock the state file to prevent concurrent change corruptions. The remote back end will remove the state file from access every time an operation occurs, automatically implementing state locking as needed.

To create a remote backend with Amazon S3, here is an example configuration:
`
terraform {
  backend "s3" {
    bucket = "gageperrin-terraform-state-bucket01"
    key = "finance/terraform.tfstate"
    region = "us-east-1"
    dynamodb_table = "state-locking"
  }
}
`

Again, state files should not be edited directly but through commands.

* `terraform state list` lists resources.
* `terraform state show` will get detailed information about a matching resource.
* `terraform state mv` will move items in a Terraform state file.
* `terraform state pull` to pull the state file from the backend.
* `terraform state rm` to remove a resource.

## Terraform Provisioners

Terraform provisioners provides the ability to run scripts or commands through the configuration file on the provisioned resources. This is done via Remote Exec. Inside a resource block, the following argument would be inserted:
`
provisioner "remote-exec" {
  inline = [ "sudo apt update",
             "sudo apt install nginx -y",
             "sudo systemctl enable nginx",
             "sudo systemctl start nginx",
           ]
  }
`

Local Exec provisioner can be used to run tasks on the machine where the Terraform binary is installed.

The Remote and Local Exec provisioners run tasks when a resource is created by default. However, this can be configured to run before a resource is destroyed or when a `terraform apply` fails, for example.

Use Terraform provisioners sparingly because there is no provisioner information in a plan, it requires pre-established network connectivity and authentication, and it scaffolds around the configuration. Bake necessary configuration into the container image beforehand, not in a Terraform provisioner.

## Terraform Import, Tainting Resources and Debugging

**Terraform Taint**

Whenever a resource creation fails, Terraform marks the resource as tainted. The next time a `terraform apply` is run, Terraform will attempt to replace the tainted resource. A taint can be reverse using `terraform untaint`.

**Debugging**

Logs provide the best deeper detail for investigating issues. Run `export TF_LOG=[LEVEL]`. There are five form levels: info, warning, error, debug, and trace. Trace includes internal Terraform malfunctioning that can be reported as a bug.

**Terraform Import**

Use `terraform import <resource_type>.<resoruce_name> <attribute>` to bring a third party resource under Terraform's jurisdiction. Terraform does not update the configuration during an import, only the state file.


## Terraform Modules

Terraform modules organize large infrastructural projects into discrete groups that are useful for configuration resource sorting and collaborative workloads. To invoke a module in a configuration file, use the following syntax:
`
module "dev-webserver" {
  source = "../aws-instance"
}
`

To create a re-usable module for an AWS configuration for instance, generate a directory, with individual Terraform files for each AWS service. The instance-type should be hard-coded while the AMI and region tags should be variable dependent.

To invoke this module, create a `main.tf` Terraform file in another directory to invoke the module as a resource with arguments `source = "../[directory]"`, `app_region = us-xxxx-x`, and `ami = xxxxxx`.

Many modules are stored in the Terraform Registry and can provide configurations for many common scenarios alongside provisioning instructions. Submodules are also contained within the Module Registry for different kinds of traffic or other basic variations.


## Terraform Functions, Conditional Expressions and Workspaces

**More Terraform Functions**

`terraform console` boots a console to test values, functions, and interpellations. Commands available in the Terraform console include `max`, `min`, `ceil`, `split`, `lower`, `upper`, `title`, `substr`, `join`, `length`, `index`, `element`, `contains`, `keys`, `values`, and `lookup(map, key)`.

**Conditional Expressions**

The Terraform console can also take in conditional expressions that can include operators of equality, comparison, or logical types.  If statements are also available as well as ternary expressions.

**Terraform Workspaces**

Terraform workspaces can be used to replicate project use cases within a single directory rather than having to replicate to an entirely separate directory.

To start a workspace, run `terraform workspace new [project]`. Run `terraform workspace list` to see workspaces available in the directory. To get the name of the current workspace, run `terraform.workspace`. 

`terraform.workspace` can be added as the name tag in the configuration file. The AMI can be specified through `ami = lookup(var.ami, terraform.workspace)`.


