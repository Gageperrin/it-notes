# Puppet Fundamentals
These are my notes from the KodeKloud official course for Puppet.

Chef Documentation can be found [here](https://puppet.com/docs/).

Puppet is Ruby based with two deployment models: Standalone and Master-Agent. Puppet Master supports Linux operating systems while Puppet Agent supports Linux and Windows.

## Introduction to Puppet

Imperative programming style explicitly specifies what to do and how to do it. Declarative programming only specifies the desired end result. Puppet is declarative.

Puppet deployment starts on a master station with target agent machines. The agent nodes check in with the master every 1800 seconds (by default) to see if changes are needed to its configuration. Puppet is a pull-based configuration in contrast to push-based deployment models like Ansible or Saltstack. Connections between masters and agents is secured through certificate, and it occurs through port 8180 on the master.

Agents send configuration facts to the master. The master contains a list of configuration requirements known as a catalog. The catalog is the reference used to apply configuration changes. If an agent receives the catalog, it will immediately apply it and upon completion send the master a report.
