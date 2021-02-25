# HashiCorp Vault

## Overview

Hashicorp currently offers several technologies that server different purposes:
* Vault - Security
* Terraform - Provisioning
* Consul - Connection
* Nomad - Running
* Sentinel - Policy

HashiCorp Vault is a platform to secure, store, and tightly control access to tokens, passwords, certificates, encryption keys for protecting sensitive data, and other secrets in dynamic infrastructure. In short, it manages secrets and protects sensitive data such as user credentials, API keys, certificates, token, etc.

Vault is designed to be a centralized identity management solution across multiple platforms whether on-premises on across cloud providers. It stores long-lived, static secrets, dynamically generates secrets upon request, acts as a root or intermediate certificate authority, provides encryption as a service, offers extensible architecture, and is a single binary.

Features:
* Key/value store
* Wide variety of secret engines
* Manage leases on secrets
* Revoke leases based on lease period
* Manage access to secrets using policies
* Namespaces - Vault as a service
* Audit devices which logs all interactions
* Encryption as a Service
* Generate dynamic X.509 certificates (PKI)
* Built-in High Availability

Open Source Features:
* Dynamic secrets
* ACL templates
* Init and unseal workflow
* Vault agent
* Key rolling
* Access control policies
* Encryption as a Service
* AWS, Azure, and GCP auto unseal

Enterprise features:
* Disaster recovery
* Namespaces
* Replication
* Read replicas
* HSM Auto unseal
* MFA
* Sentinel
* FIPS 140-2 and seal wrap

Vault components:
* Storage backends
* Secrets engines
* Authentication methods
* Audit devices

Everything in Vault is path based. The path prefix tells Vault which Vault component a request should be routed to. Secret engines and authentication methods are mounted at a specified path. Paths available are dependent on the features enabled in Vault such as Authentication Methods and Secrets Engines. System backend is a default backend in Vault which is moutned at the `/sys` endpoint. Permissions or policies are granted based upon the path. Some secret engines and authentication methods have preconfigured paths.

## Data Protection in Vault

### How does Vault protect data?

Vault first creates an encryption key to encrypt Vault data, before that key is encrypted by a master key. The encrypted encryption key is stored with vault data. The master key is stored in the Vault node.

### Vault initialization, seal, and unseal

Initializing Vault prepares the backend storage to receive data. It only needs to initialize a Vault cluster via a single node. Vault initialization is when Vault creates the master key. It includes options to define thresholds, key shares, recovery keys, and encryption. Vault initialization is where the root token is generated and provided. To initialize Vault, run `vault operator init`.

Vault starts in a sealed state which means it knows where to access data but is unable to decrypt it. The only operations possible when Vault is in a sealed state are status check and unsealing. Unsealing Vault means that a node can construct the master key in order to decrypt and read the data. The master key is only stored in memory and is thrown away when Vault is sealed. Vault can also be manually sealed via UI, CLI or API when key shards are inadvertently exposed or if you detect a network intrusion.

### Unsealing Vault

Vault can be unsealed in three different ways:
1. Key sharding
2. Cloud auto unseal
3. Transit auto unseal

To unseal with key shards, the master key should be divided into multiple pieces using Shamir's Secret Sharing Algorithm. Each shard should be allocated to different employees with security responsibilities (`-key-shares=`). To unseal the vault, a specified threshold amount of key shards are required from different members to unseal the Vault (`-key-threshold=`). This is the default option for unsealing and does not need configuration. When initializing Vault, it can be requested to have the individual shards be encrypted with different PGP keys.

To unseal with auto unseal, the master key should be stored in a KMS solution. Auto unseal can use a cloud or on-premises HSM or other Vault cluster to unencrypt the master key and unseal Vault. The Vault configuration files should identifiy a particular key to use for decryption. Cloud auto unseal automatically unseals Vault upon service or node restart without additional intervention. It is available in both open source and Enterprise editions.

AWS KMS example syntax:
```
seal "awskms" {
  region = "REGION"
  kms_key_id = "KMSKEY"
}
```

To unseal with transit auto unseal, Vault Transit on another Vault cluster is used to encrypt the master key. A core Vault cluster is used to seal a variety of other Vault clusters. The Transit Secret Engine may be configured in a namespace. The Transit unseal supports key rotation, and it is available in both open source and enterprise. However, the core Vault cluster must be highly available.

Transit auto unseal configuration values include `address` for the Vault Transit cluster, `token` for an ACL token, `key_name` for the Transit key used for encryption or decryption, `mount_path` for the mount path to the Transit Secret Engine, and `namespace` for the namespace path to the Transit Secret Engine.

Key shards are the simplest form of unsealing, flexible configuration options, and work on any platform. However, it introduces risk for storing keys and requires manual intervention for unsealing. Keys can be accidentally shared and then rotation is needed.

Auto unseal automatically unseals Vault with a set and forget workflow. It offers integration benefits for running on the same platform. However, it has regional requirements for cloud HSMs. It has a cloud lock-in.

Transit unseal also automatically unseals Vault with a set and forget workflow. It is platform agnostic and is useful when running many Vault clusters across many platforms. But it does require a centralized Vault cluster that needs the highest level of uptime.

## Vault Configuration

### Storage Backends

Storage backends configure the location for the storage of persistent Vault data. Storage is configured in the main Vault configuraation file in the storage stanza along with desired parameters. Not all storage backends are created equal. Some support high availability and others have better tools for management and data protection. The only HashiCorp supported backends are Consul, Raft, in-memory, and file. Storage backends are configured with a `storage` stanza in a Vault configuration file where `ha_enabled` is set to enabled if necessary.

The Consul client is run on the Vault node at port 8500, and the client is connected to a quorum of Consul servers. To back up Vault, you need to back up the storage backend. Vault nodes can be treated as immutable. 

To restore Vault, it is necessary to restore the storage backend. It is possible to deploy a new storage backend and Vault cluster. First the backup is retrieved, then a new backend is deployed, the restore process is started, it launches Vault, provides the necessary configuration, and then unseals it.

### Vault Configuration File

Vault servers are configured using a file written in HCL or JSON. The configuration file includes different stanzas and parameters to define a variety of configuration options. Configuration file is specified when starting Vault using the `--config flag`. The configuration file is usually stored in `/etc`.

The Vault configuration file includes a storage backend, listener and ports, a TLS certificate, a seal type, a cluster name, a log level, UI, cluster IP and port. 

It does **not** include Secrets Engines, authentication methods, audit devices, policies, entities and groups.

Configuration files are composed of stanzas that specify an option with embedded paired parameter names values. Stanza types include `seal`, `listener`, `storage`, and `telemetry`. Parameters include `cluster_name`, `log_level`, `ui`, `api_addr`, and `cluster_addr`. Note that `tcp` is the only option ofr the listener stanza.

## Deploying Vault

### Dev Server Mode

Dev Server mode does the following:
* Quickly run Vault without configuration
* Automatically initialize and unseal
* Enables the UI, available at localhost
* Provides an unseal key
* Automatically logs in as root
* Non-persistent, runs in memory
* Insecure, does not use TLS
* Sets the listener to 127.0.0.1:8200
* Mounts to a K/V v2 Secret Engine
* Provides a root token

Dev Server mode should be used for proof of concept, testing and experimenting with features, and new development integrations. It should never be used in a production environment. To use Dev Server mode, one has to download Vault from HashiCorp, unpackage it to a directory, and set a path to the executable. Use `vault server -dev`.

### Production Deployment

Deploy one or more persistent nodes via a configuration file. Use a storage backend that meets the requirements. `MultipleVault` nodes should be configured as a cluster. Deploy close to applications. Most likely this provisioning process will be automated.

### Configuring Consul Storage Backend

Consul provides durable key-value storage for Vault, supports high availability, can independently scale the back-end, is a distributed system, easy to automate, and includes built-in snapshot functionality. It includes built-in integration between Consul and Vault and is officially supported by Hashicorp.

Consul is deployed using multiple nodes and configured as a cluster. Clusters are deployed in odd numbers to maintain quorum. All data is replicated among all nodes in the cluster. A leader election promotes a single Consul node as the leader. The leader accepts new logs entries and replicas to all other nodes. Consul cluster for Vault storage backend should not be used for Consul functions in a production setting.

A Consul cluster takes an application write to the Vault cluster, stores it key-value format to the storage backend. The consul leader node will accept the key-value write and will replicate it to the follower nodes.


## Accessing Vault

Vault has three interfaces: UI, CLI, and HTTP API. Not all features are available through the UI or CLI, only the API can access all features. Most calls from teh CLI and UI invoke the HTTP API. UI must be enabled via the configuration file. Authentication is required to access any of the interfaces.

Access UI through port 8200 by default. It runs on the same IP as the listener. Vault UI enables access to configure the key-value store, secrets engine, ACL policies, authentication methods, direct CLI access, replication (enterprise), licensing (enterprise), as well as groups and entities.

Access the CLI through the Vault node or a remote machine (needs to be authenticated through `vault login`). Need to set the environment variables `VAULT_ADDR` if different than the default `https://127.0.0.1:8200`. Use `vault -autocomplete-install` to enable auto-completion. Use `vault -h` to get help.

API access is provided through REST API or by using `curl` or any other http client. It is necessary to authenticate first, and Vault will return a Vault API token that cna be used for future requests.


## Securing Vault with Policies

It provides a way to permit or deny access to certain paths or actions within Vault (RBAC), authorization using a declarative policy written in HCL or JSON. Permissions include create, `read`, `update`, `delete`, `list`, `deny`, and `sudo`. 

Policies are deny by default. The simplest form of policy includes a path and the permitted capabilities. More complicated policies can include variable replacmeents and/or parameters. Policies are cumulative and capabilities are additive. Deny always takes precendence over other capabilities. UI access may require additional permissions.

A user is authenticated with credentials through Vault which verifies the credentials with the LDAP server. It creates a token and associates a policy to it before returning it to the user. There are two default policies in Vault: Root and Default. The root superuser cannot be changed or deleted. There is a common policy assigned to all tokens. Common policies include Vault administrator, security team, developer, database administrator, application owners, and help desk.

When approving new policies, the requesting team creates a new policy, the requesting team submits the PR, then the security team approves the policy, and the policy is applied via automation.

Example:
```
path "secret/apps/application1/app-secret"
{
  capabilities = ["read", "update"]
}
```

The glob (`*`) is a wildcard can can only be used at the end of a path to signify anything after a path or as part of a pattern. The plus sign denotes any number of characters within a single path segment. The plus is used when it does not matter what a middle segment is. Furthermore, the plus sign can be used in multiple path segments.

The `"deny"` capabilities can be used to provide a more granular deny within a directory where other actions have been approved.

Administrative policies are permissions for Vault backend functions live at the `sys/` path. Users and admins will need policies that define what they can do in order to administer Vault itself.

If using the UI, it is necessary to add additional `LIST` permissions in order to browse various paths. `LIST` does not allow the user to read secrets. The lack of `LIST` permissions is security by obscurity.


## Auth Methods

Auth methods are components that perform authentication to Vault itself, responsible for assigning identity and policies to a user. Multiple auth methods can be enabled depending on use case. Auth methods are enabled at a path commonly using the same name as the auth method but not required. Default authentiation method for a new installation is through tokens.

Auth methods:
* AppRole
* Azure
* JWT
* LDAP
* RADIUS
* AliCloud
* Cloud Foundry
* Kubernetes
* Okta
* TLS Certificates
* Username & password
* AWS
* GCP
* GitHub
* Oracle Cloud
* Tokens

The authentication flow consists of:
1. Authentication with credentials
2. Verification of credentials
3. Policy association
4. Return token to user

### Tokens

The default auth method consists of tokens. Root token is assigned the root policy upon Vault initialization. The root token should not be used on a day-to-day basis only for initial authentication. Instead create an admin account and delete the root token using `vault token revoke`.

Tokens are the core authentication method within Vault. They can be sued directly or generated by another auth method. Vault verifies identity and then generates a unique token to associate with that identity for future requests. the CLI and UI automatically attach this token, but the API requires this to be manually done. A token accessor is created alongside the token and serves as reference to the token. The token accessor can be used to look up token capabilities, look up token properties, and revoke the token.

Non-root tokens have an associated TTL after which is it is revoked. Tokens can be renewed, if permitted with `vault token renew`. Tokens can have a hard limit max TTL as well.

When a token is created, it is the child of the creator. If the parent token is revoked or expired, the child tokens are all revoked as well. Parent is almost always a token. Child can be a token, secret, or auth created by the parent.

Service tokens are normal tokens that are used in most cases. Batch tokens are encrypted blobs with just enough info to be used with Vault. Batch tokens are lightweight and scalable but lack flexibility. They are used for ephemeral, high performance workloads because they do not require storage on disk.

### LDAP Auth

LDAP is one of the most frequently used Auth methods in enterprise situations because it is so common in on-premises environments. It is mostly used for user authentication. It points to LDAP servers for authentication. It can supply multiple servers and will be used in order. It identifies the user attribute and configures group filter and group DN for searching group membership. Once configured, the user maps a group to an AD group and assigns a Vault policy.

### Userpass Auth 

Not often seen in enterprise situation but can be used for proof of concept or demos. It starts up a local database of users and passwords. It cannot read usernames and passwords from an external source.

### AppRole Auth

This method is ideal for machine to machine communication. It is basically a username and password for machines. It consists of a `RoleID` which is not sensitive data and can be embedded in an AMI or Dockerfile as well as a `SecretID` which is the credential required for login and is sensitive. These two are combined to create a Vault token.

## Secrets Engines

There are several problems with static secrets. They never expire, are often shared among team members without accountability, are valid 24/7, rarely rotated, and live perpetually. Static secrets are easy to use but pose several problems.

Dynamic secrets by contrast can be created on-demand and are not stored independently. They have associated time-based or use-based leases. They have a validity period and solve technical debt problems through their TTL hierarchy. They can be renewed with granularity and revoked much more easily than static secrets.

Dynamic secrets can be used in CI/CD pipelines, database credentials, privileged access for administrators, and elevated account for vulnerability scanning.

### Vault Secrets Engines

Secrets engiens are the reason Vault is deployed. They can store, generate, or encrypt data. They are enabled and isolated at a path and all interactions are done directly with the path itself. There are a large number of supported Secrets Engines.

Key Value secrets engine creates a foundational structure that uses parameters to simplify policies for administration, groups by application and teams, creates additional mounts, and provide a different structure for each case (though these should be standardized across environments).
