# Chef Fundamentals

These are my notes from the KodeKloud official course for Chef [found here](https://kodekloud.com/courses/chef-for-the-absolute-beginners/).

Chef Documentation can be found [here](https://docs.chef.io/).

## What is Chef?

Chef is a Ruby based system automation tool used to deploy, manage, and orchestrate various infrastructures. It comes in two deployment models: Chef-Zero and Server-Client. The former is used for development purposes while the latter is used for testing or proof-of-concept. The Chef Server is Linux only while the Chef Client can use Linux or Windows.

Chef is declarative, highly scalable, provides consistent productivity, and idempotent. 

It can be used to set up a web-server, databases, set up firewall rules, or even configure tools. It can also be used with hybrid cloud deployments.

## Chef Concepts

The Chef system is divided into three entities: the Chef workstation, the Chef server, and the Chef ClientNode. The Chef Workstation is used to create, validate, and apply recipes. This is where development work occurs. Chef Workstations are connected to the Chef server through RSA Key-Pairs and communicate through Knife. Knife is a CLI utility. A node is any virtual, cloud or network machine that has `chef-client` installed on it. The Chef server uses `OHAI` to get information from the nodes whenever it is started.

Chef resources are the building blocks of Chef code. Multiple resources are grouped together into a single recipe. Multiple recipes are collected into cookbooks. They are re-usable and shareable as directories. A runlist determines the order of execution for various cookbooks and recipes.

### Chef Resources

Chef resources are ready made tools that can perform various tasks and operations on a wide range of platforms. Resources can be built-in (e.g. user) or custom, written in Ruby.

Chef DSL (Domain Specific Language) is the language that Chef resources are written in. There are several fundamental parts to a Chef DSL resource: the resource type, name, start block, attribute name, attribute value, the action, and the end closing block.

`
file '/var/www/html/index.html' do
  content "This is content in file\n"
  action :create
end
`

Resource types include `package`, `file`, `service`, etc.


### Chef Recipes

To make a recipe, create a file with the `.rb` file extension. Use `cookstyle <RECIPE FILE>` to check the code then test it with `chef-clietn --local-mode --why-run <RECIPE FILE>`. Then run it using `chef-client --local-mode <RECIPE FILE>`.

Chef client is a local agent on each node that ensures that its resident node conforms to the specifications given by the Chef Server. Local mode instructs the utility to test and apply the recipe on a local mode. Chef-Zero is best for this kind of testing and development.

### Chef Cookbooks

All Cookbooks share a basic file structure. The `recipes` directory stores all the recipes with a `default.rb` configuration. The default path for the cookbooks directory can be found in the `.chef` directory and changed inside that directory's `knife.rb` file.

Run `chef generate cookbook [NAME]` to create a cookbook. A cookbook can follow the same creation process as a recipe. Use `chef-client --local-mode` to run a cookbook locally.

### Chef Runlists

RunLists provide a sequence of recipes to be run on each node. Syntax: `chef-client --runlist "recipe[Cookbook-Name::Recipe-Name]"`.

A RunList could, for example, generate a cookbook skeleton, check the syntax, conduct a dry-run test, then a real-run test before provisioning from the recipe. Runlists can also run multiple recipes at once.

### Other Topics

**Include Recipe**

For larger recipes, it may be better to split a recipe up into multiple files and invoke each component with the `include_recipe` directive in the main recipe file.

**Dependencies**

Dependencies can be used to link other cookbooks through using the directive `depends '[cookbook]'`.

**Server-Client Model**

Code is developed on the individual Workstation before it is uploaded to the Chef server where cookbooks and recipes are stored. Knife is the utility on the individual workstation that interacts with the server, and it uploads the code when properly configured to communicate with the server.

Runlists are created on the server to provide an explicit sequence for recipe and cookbook implementation. These are generated through the command `knife node run_list add NODE_NAME RUN_LIST_ITEM`.

These runlists are then deployed on the nodes by executing `knife ssh 'name:*' 'chef-client'` to provision the runlist on each node.

Other commands:
* `knife ssl check` checks chef-server connectivity from the workstation
* `knife cookbook upload cookbook names` to upload the cookbook onto the Chef server
* `knife cookbook list` to check all uploaded cookbooks.

**Ohai**

Ohai is a tool used to collect system configuration data that is run by the chef client to determine the current system state. Data from Ohai is returned in JSON format.

**Kitchen**

Kitchen can be used for testing by creating a virtual machine, installing chef tools, copying and applying the cookbooks, before cleaning it up.

**Roles**

Runlists can be grouped into roles that provide greater abstraction and organization for recycled processes.

**Environment**

Environments can provide mirroring between development and production spheres while also providing isolation between them. Chef can be used to provision environments with specific attributes that can be used to filter node placement.

**Supermarket**

Chef Supermarket is the marketplace for Chef cookbooks, plugins, and other resources.
