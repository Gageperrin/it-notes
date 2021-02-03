# Caching, Delivery and Edge

## Table of Contents

1. [CloudFront Architecture](#cloudfront-architecture)
2. [TTL and Invalidations](#ttl-and-invalidations)
3. [CloudFront SSL and SNI](#cloudfront-ssl-and-sni)
4. [Origin Types and Architecture](#origin-types-and-architecture)
5. [Caching Performance and Optimization](#caching-performance-and-optimization)
6. [CloudFront Security](#cloudfront-security)
7. [Lambda@Edge](#lambda@edge)
8. [ElastiCache](#elasticache)

## CloudFront Architecture

> [Amazon CloudFront](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html) is a web service that speeds up distribution of your static and dynamic web content, such as .html, .css, .js, and image files, to your users. CloudFront delivers your content through a worldwide network of data centers called edge locations. When a user requests content that you're serving with CloudFront, the user is routed to the edge location that provides the lowest latency (time delay), so that content is delivered with the best possible performance.

CloudFront is a content delivery network (CDN) which has an (S3 or custom) origin. The distribution is the configuration unit. The edge location is the local cache of data. There is also a regional edge cache which is a larger version of an edge location.

CloudFront behaviors control the TTL, protocol and privacy settings in CloudFront. The price class is based on which edge lcoations are used. WAF web ACLs are configured at the distribution level. Alternate domain names and custom SSL are configured here as well. Can select the security policy to use (balance between security and browser accessibility). A single distribution can have multiple behaviors with one default. Behavior precendence based on priority numbers which starts at 0 as the default. Higher numbers have higher priority.

Behavior configuration includes:
* Origin
* Viewer protocol policy (HTTP/S)
* Allowed HTTP methods
* Field-level encryption configuration
* Caching directives
* It can restrict viewer access per behavior (i.e. trusted signers for each behavior)
* It can set to compress objects automatically.


## TTL and Invalidations

Cached images can potentially return outdated resources if origin is updated. When the cached object expires and is requested again, a 304 (not modified) is returned if it is not changed. If it was changed, a 200 (OK) is returned with the new object. More frequent cache hits means a lower origin load. Default TTL behavior (validity period) is 24 hours. Can set minimum and maximum TTL values and even set TTL per object. 

The origin header can set a cache-control max-age in seconds. There is also a cache-control s-maxage in seconds. It can also be set to expire at a specific date and time. Values below the minimum or above the maximum TTL will use the minimum or maxium. Cache invadliation is performed on a distribution and applies to all edge locations but it takes time. Invalidation should only be used to correct errors.

It is better to use versioned file names instead. Objects cached in a customer's browser will not be impacted. It also means logging is more effective. All versions of all objects will be maintained which means better consistency across edge locations. This is not S3 versioning as there are different file names.


## CloudFront, SSL and SNI

Each CloudFront distribution receives a default domain name (CNAME record). SSL supported by default `*.cloudfront.net` Can have alternate domain names. CloudFront verifies ownership using a matching certificate. It generates or imports from ACM (**always in us-east-1**) for CloudFront. Can faciliatate HTTP/S, HTTP -> HTTPS, or HTTPS only.

There are two SSL connections through CloudFront: Viewer -> Cloudfront and CloudFront -> Origin. Both connections need valid public certificates (self-signed certifications do not work). Historically (before 2003) every SSL-enabled site needed its own IP. Encryption starts at the TCP connection. Most headers happen after that Layer 7 / Application. For TLS, this encryption is before the header happens.

In 2003, a TLS extension named SNI was added to allow a host to be included. Results in many SSL Certs/Hosts using a shared IP. Old browser don't support SNI. CloudFront charges extra for a dedicated IP ($600/month).

Architecture: Viewer protocol is the connection between the user and the edge location. The certificate used by the edge location must be a trusted certificate authority such as Comodo, DigiCert, Symantec, or ACM (us-east-1) which matches the DNS name. Origin protocol must also use publicly trusted certificates. ALB can use ACM but no self-signed certificates. S3 Origins handles certificates natively. Custom origin EC2 or on-premises is not supported by ACM, so it is necessary to apply certificates manually.


## Origin Types and Architecture

When there are two or more origins in a distribution, they can be put into an origins group to provide re-routing in case of a failover event. Origins are selected from a behavior. They are split between S3, Media Package, Media Store, and everything else (web servers). If S3 is configured as a static website, CloudFront will treat it as a web server.

S3 Origins is the simplest to integrate. It can configure origin domain name, path, bucket access restrictions, origin access identity (CloudFRont can be given an identity to access S3 Origins), read permissions on a bucket, and origin custom headers. S3 matches viewer and origin protocols.

Custom origins are more complicated. It is necessary to specify origin domain name, origin path, origin ID, minimum origin SSL protocol, origin protocol policy, origin connection attempts/timeout/response timeout/keep-alive timout, HTTP/S ports (default 80 and 443 respectively), and origin custom headers.


## Caching Performance and Optimization

A cache hit means there is a matching object that is delivered from the cache. A cache miss means there is a matching object not cached. It is best to maximize the cache hit ratio for best performance.

The request includes the object name, query string parameters, cookies, or request headers. These are sent to the Edge location with CloudFront. CloudFront can be configured to choose which of these should be forwarded and if CF Cache should be based on all or selected parts of requests. The cache will forward only what the application needs to the origin. The more entities involved in caching, the less efficient the process.


## CloudFront Security

### OAI and Custom Origins

The main practices to secure origins from direct access are Origin Access Identities (for S3 Origins only) , custom headers, and IP based FW blocks. An OAI is a type of identity that can be associated with CloudFront distributions. CloudFront becomes the Oai, and this can be used in S3 bucket policies (deny all but some OAIs).

To secure custom origins, the user can create custom headers that are injected at the Edge location. Alternatively, a traditional firewall can be set up around the custom origin to block everything that is not in the CloudFront IP range.

### Private Distributions

Public distributions are open access, whereas private requests require signed cookies or URLs. Each behavior can be public or private, allowing for pure or mixed distributions. A CloudFront key is created by an account root user, the account with that key is added as a trsuted signer. Signed URLs provide access to one object and should be used if the client cannot support cookies (e.g. legacy RTMP distributions).

Cookies provides access to groups of objects. Use for groups of files/all files of a type. Also good to use if maintaining URLs is important.

### Geo Restriction

There are two common architectures: the built-in CloudFront Geo Restriction or a 3rd party Geolocation service. CloudFront can whitelist or blacklist based on **country only**. GeoIP Database is 99.8%+ accruate and applies to the entire distribution.

CloudFront can interact with the application, ensure the requested object is directed to closest edge location, then the edge location checks if geo restriction is enabled by consulting the the GeoIP database. If there are restrictions, it returns a 403 error.

A 3rd party service requires that an app server and the CloudFront distribution be configured as private. It works using signed URLs and cookies returned from the app server to the device and then forward to the CloudFront Edge location.

### Field-Level Encryption

The client and origin relationship can be encrypted using HTTPS. Information is processed at the web server as HTTP plaintext. Field level encryption happens at the edge, separately from the HTTPS tunnel. A private key is needed to decrypt individual fields. The public/private key pair is stored at the Edge location, so data is encrypted at the Edge and remains separately controlled throughout its lifecycle.


## Lambda@Edge

Lambda@Edge allows CloudFront to run Lambda functions at Edge locations to modify traffic between the viewer and edge location as well as between the edge locations and origins. It currently oonly supports Node.js and Python. It is run in the AWS public space (not a VPC). Layers are not supported though, and they have different time limits than normal Lambda functions.

There are four stages for when a Lambda function can be run:
1. Viewer request
2. Origin request
3. Origin response
4. Viewer response

On the viewer side there is a 128 MB and 5 second timeout limit. On the origin side there is the normal Lambda MB limit and a 30 second timeout.

Use cases: A/B testing for viewer request, migration between S3 Origins through origin request, different objects based on device through origin request, content variation by country.


## ElastiCache

> [Amazon ElastiCache](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/WhatIs.html) is a web service that makes it easy to set up, manage, and scale a distributed in-memory data store or cache environment in the cloud. It provides a high-performance, scalable, and cost-effective caching solution. At the same time, it helps remove the complexity associated with deploying and managing a distributed cache environment.

ElastiCache is a managed in-memory cache which provides a memory implementation of the Redis or Memcached engines. It is useful for **read heavy workloads**, scaling reads in a cost-effective way and allowing for externally hosted user session states. It is high performance with **low latency**. It reduces database workloads and can be used to store session data. However, it does **require application code changes** for implementation. It is fault tolerant in case of database failure as the session state is loaded from the cache.

Use Redis for:
* Advanced data structures
* Multi-AZ
* Replication (scale reads)
* Backups and restores
* Transactions

Use Memcached for:
* Simple data structures
* No replciation
* Multiple nodes (sharding)
* Multi-threaded workloads (utilize multi-core CPUs)
