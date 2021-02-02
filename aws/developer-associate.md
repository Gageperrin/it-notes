# AWS Certified Developer Associate Notes

These are my notes for the AWS DVA-C01 exam. I have taken the content from [Adrian Cantrill's course](https://learn.cantrill.io/p/aws-certified-developer-associate) for this exam, Tutorial Dojo's practice tests, and the official AWS documentation. 

These notes assume full and prior knowledge of requisite material for the Solutions Architect Associate and Professional exams and will not mention or cover any topics that are featured in those exams. Consequently, only a few topics will be treated here.

## CloudFormation

CloudFormation templates are in either YAML or JSON and contain logical resources used to create stacks of physical resources.

Template parameters accept input when a stack is created or update, and these can be referenced from within logical resoruces. They influence physical resources and/or configuration. They can create defaults, `AllowedValues`, min and max, `AllowedPatterns`, `NoEcho`, and have a type. Pseudo Parameters are injected by AWS into the CloudFormation Stack (e.g. `AWS::Region`).


### Intrinsic Functions

Intrinsic Functions can be used to assign values to properties that are not available until runtime. For example:
* `Ref` and `Fn::GetAtt` can be used to reference resources in another template or stack. 
* `Fn:Join` and `Fn::Split` joins and splits strings. 
* `Fn:GetAZs` and `Fn::Select` are commonly used together to pick an AZ out of a list of available AZs. 
* Conditional operators
* `Fn::Base64` and `Fn::Sub` alter the inputted string.
* `Fn::Cidr` can configure CIDR ranges automatically.

`Ref` can be used to reference the main value of a parameter or logical resource. Using `!Ref` on template or pseudo parameters returns their value. When used with logical resources, the physical ID is usually returned.

`!GetAtt LogicalResource.Attribute` can be used to retrieve any attribute associated with the resource. This includes detailed configuration of the physical resource.

`Fn::GetAZs` returns the list of AZs in a region where subnets exist for that VPC. This is every AZ by default but a badly configured VPC may not have all AZs. These can be selected with `Fn::Select`.

`Fn::Base64` accepts plaintext and outputs Base64 encoded text. It substitutes variables in the input.

`Fn::Cidr` automates subnet range deployment and can specify how many subnets to generates and how many bits for each range. This can be combined with `Fn::Select` to assign to AZs.

### Mappings, Outputs, Conditions, DependsOn, and Wait Conditions

Templates can contain a Mappings object which can contain many mappings that map keys to values. These can be looked up from a key or from a top or second level (e.g. a top key for an AMI would be region). Mappings use the `!FindInMap` intrinsic function. They are commonly used for retrieving AMI data.

Syntax: `!FindInMap [ MapName, TopLevelKey, SecondLevelKey ]`
Example: `!FindInMap [ "RegionMap", !Ref 'AWS::Region', "HVM64" ]` finds HVM64 AMI architecture.

Templates can have an optional outputs section that can be visible from the CLI, the console UI, can be accessible from the parent stack when nesting, and can be exported for cross-stack references.

Conditions are created in the optional `Conditionals` section of a template, and they are processed before resources are created. Use other intrinsic functions associated with logical resources to control if they are created or not.

`DependsOn` helps organize a dependency order for CloudFormation because CF does things in parallel by formally defining dependencies. This is mainly useful for making CF templates more readable. 

Note that an elastic IP requires an IGW attached to a VPC in order to work, but there is no dependency in the template. Without an explicit dependency, CF will delete resources in a random order, and this can cause errors depending on the sequence it creates IGW and the EIP. An explicit depends on can prevent this.

With simple provisioning, once the logical resource reaches a `CREATE_COMPLETE` state then no other information from the resource is used in the CF provisioning stage, even if this is long before the bootstrapping is complete.

However with CF Signal, it is possible to hold, wait for a certain number of success signals, or wait for a timeout for these signals of up to 12 hours. If there is a failure signal, then creation fails. If timeout is reached, then creation fails. `CreationPolicy` or `WaitCondition` can help with this process.

`CreationPolicy` applies signal or timeout requirements for particular resources before a `CREATE_COMPLETE` state is reached. `WaitCondition` is a specific logical resource of its own. It can have dependencies or be depended upon. It has a `WaitHandle` that can pause provisioning for certain resources while waiting for other resources to return JSON data. It can also query this returned data in other resources using `!GetAtt WaitCondition.Data`.

### Nested Stacks, Cross-stack, and StackSets

A single stack has a hard limit of 500 resources, and resources in a single stack share a lifecycle. Stacks can't easily resuse resources or reference other stacks.

Nested stacks have an ultimate root stack (manually created) and a parent stack for each child stack. Child stacks can have a sequence of `DependsOn` to provide order when provisioning, providing outputs to one another. Whole templates can be reused in other stacks. Use nested stacks when they form part of one solution and have a linked lifecycle.

Cross-stack references are designed to be isolated and self-contained so they are not lifecycle linked. They are helpful because outputs are not normally visible from other stacks. Outputs can be exported to make them visible from other stacks. Exports must have a unique name in the region. `Fn::ImportValue` can be used instead of `Ref`. Every region in an account has its own list of exports. `ImportValue` can be used instead of `Ref` to reference region exported values. Cross-stack references are service oriented, feature stack reuse, and permit different lifecycles.

StackSets allow for deploying CFN stacks across many accounts and regions. StackSets are containers in an admin account. StackSets contain stack instances which reference stacks (they are not just stacks, stack instances remain even if a stack fails). Stack instances and stacks are in target accounts, and they can be self-managed or service-managed. StackSets can specify concurrent accounts, failure tolerance, and retention policies. They can be used to enable AWS Config, Config rules, or create IAM roles.

### Deletion Policy

If a logical resource is deleted from a template, then the physical resource is deleted by default which may accidentally cause data loss. A deletion can be set to `Delete`, `Retain`, or sometimes `Snapshot`. This determines what happens when a logical resource is deleted. The deletion policy does not apply to a replace action where resources are recreated.

### Stack Roles

When a stack is created, CFN creates physical resources. CFN uses the permissions of the logged in identity which requires AWS permissions. CFN can assume a role which does not need resource permissions only `PassRole`.

### `cfn-init` and `cfn-hup`

`cfn-init` is a simple configuration management system. Its configruation directives are stored in a template. `AWS::CloudFormation::Init` is part of a logical resource and is an imperative approach. By contrast, `cfn-init` is declarative and provides the desired state.

`cfn-init` helper script installed on the EC2 OS makes it possible to access `cfn-init`. It communicates directly with the provisioned resources rather than only indirectly via the stack. `ConfigKeys` provide creation meta-data for packages, groups, users, sources, files, commands, and services to be included in the launched infrastructure. `ConfigKeys` can be bundled into a `ConfigSet`.

[`cfn-hup` later]

### ChangeSets

Change sets can be used to preview how changing configured resources in a stack may affect the environment. It is possible to review many different versions of a change set and execute a single one.

### Custom Resources

Custom resources allow the user to write custom provisioning logic that runs anytime a stack is created, updated, or deleted. For example, a Lambda function can be invoked to receive events and upload them to a S3 bucket.
