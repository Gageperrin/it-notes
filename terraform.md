# Terraform


## Introduction to Infrastructure as Code

Infrastructure as code provides a more robust development and deployment process. By implementing IaC through cloud providers, pipelines can be developed and deployed in a few clicks. Cloud environments provide better tools for development, deployment, implementation, testing, maintenance, and organizing workloads while reducing costs and time.

Terraform and Ansible provide simple, readable language for writing scripts and other infrastructure code. IaC tools can be categorized as configuration management (Ansible, Puppet, Saltstack), server templating (Docker, Packer, Vagrant), and provisioning tools (Terraform, CloudFormation).

Configuration management is used to install and manage software, maintain a standard structure and version control, and be idempotent. Server templating offers pre-installed software and dependencies, virtual machines or Docker images, and immutable infrastructure. Provisioning tools deploy immutable infrastructure resources such as servers, databases, network components, etc. across multiple providers. Terraform is vendor agnostic.

Terraform can be configured to work with physical machines, VMWare, AWS, GCP, Azure, alongside hundreds of other providers. Terraform uses HCL (HashiCorp Configuration Language), a simple declarative language that provisions resources through blocks of code.

Terraform works by first initalizing the project and identifying providers, then it plans how to arrive at the desired state specified in the Terraform file, and lastly applies the plan to provision the necessary resources. Every object managed by Terraform is called a resource. Terraform records the state of the infrastructure to determine changes needed to maintain the desired state. Terraform can also import resources and manage them moving forward.

## Getting Started with Terraform

### HCL Basics

