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


## Accessing Vault
