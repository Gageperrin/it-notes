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
