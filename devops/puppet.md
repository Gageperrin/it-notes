# Puppet Fundamentals
These are my notes from the KodeKloud official course for Puppet.

Chef Documentation can be found [here](https://puppet.com/docs/).

Puppet is Ruby based with two deployment models: Standalone and Master-Agent. Puppet Master supports Linux operating systems while Puppet Agent supports Linux and Windows.

## Introduction to Puppet

Imperative programming style explicitly specifies what to do and how to do it. Declarative programming only specifies the desired end result. Puppet is declarative.

Puppet deployment starts on a master station with target agent machines. The agent nodes check in with the master every 1800 seconds (by default) to see if changes are needed to its configuration. Puppet is a pull-based configuration in contrast to push-based deployment models like Ansible or Saltstack. Connections between masters and agents is secured through certificate, and it occurs through port 8180 on the master.

Agents send configuration facts to the master. The master contains a list of configuration requirements known as a catalog. The catalog is the reference used to apply configuration changes. If an agent receives the catalog, it will immediately apply it and upon completion send the master a report.

Pre-requisites:
* Deployment type (master or stand-alone)
* Persistent Hostname
* System Requirements
* Supported Platform (No Windows)
* Firewall Settings (Port 8140)
* Time Sync


An example of Puppet configuration management to add a user:
```
user { "person":

ensure => "present",

}
```

To remove this user replace `"present"` with `"absent"`.

Another example:
```
# NTP Package installation

package { "ntp":
  ensure => "present",
}

# NTP file configuration
file { "/etc/ntp.conf":
  ensure => "present",
  content => "server 0.centos.pool.ntp.org iburst\n",
}

# NTP Sservice startup
service { "ntpd":
  ensure => "running",
}
```

## Puppet Concepts

A Puppet agent sends data to the master in the form of key-value facts using the `facter` process. Puppet master compiles a catalog that specifies how the agent should be configured. The master compares the facts to the catalog and identifies drift between the expected and running state. It sends the catalog to the agent to update and provide any necessary configuration changes. Puppet can also report this information to third parties.

Puppet resources are the back-end functions that provide the base-line features of Puppet. Resources and units can be grouped into classes that can be used for a specific task. A manifests groups resources together into a `.pp` file which provides declarative code for configuration. Modules are the highest level of organization that include different manifests and classes.

Puppet resources can be a file, user, computer, router, package, or any other individual entity. These previous types are core or built-in resource types but custom resource types can be built to through Puppet declarative language. (For example: `Define apache::vhost {...}`)

### DSL - Domain Specific Language

Resource declaration syntax:
```
<Resource Type> { <TITLE>:

<ATTRIBUTE> => <Value>,
<ATTRIBTUE> => <Value>,

}
```

A resource cannot be declared twice. Compilation will fail.

Resource types each have their own attributes.

### Puppet Classes

Umbrella syntax around resources:
```
class <class-name> {

  <Resource Declarations>

}
```

### Puppet Manifests

Manifests are a tier above classes and the default path can be found using `puppet config print`. The primary Puppet file is `site.pp` located in the `/etc/puppetlabs/code/environments/production/manifests/` subdirectory. Multiple classes can be defined in the `site.pp` file.


### Node Definitions

Node definitions is a mechanism in node catalogs that allow specific configurations for individual nodes. Node definitions should be placed in the main manifest file `site.pp` to individualize configurations for each node. It is generally recommended to have no configuration details in the main manifest but rather only use it as a directory for different node definitions.

### Miscellaneous

* Variables are prefixed by a `$` and assigned a value with `=`.
* Facter is written in json-format and individual facts can be retrieved with `facter` with the sought after fact.
* Modules follow a file directory format.
* Puppet offers roles and profiles for infrastructure segmentation.
* HIERA allows the storage of secrets separated out from Puppet code.
* Puppet Forge is the official website for Puppet modules.
