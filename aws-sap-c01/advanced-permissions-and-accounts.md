# Advanced Permissions and Accounts

## Security Token Service

> AWS Security Token Service (AWS STS) is a web service that enables you to request temporary, limited-privilege credentials for AWS Identity and Access Management (IAM) users or for users that you authenticate (federated users)

STS generates temporary credentials through `sts:AssumeRole`. These credentials do not belong to the identity and expire. An external or AWS identity requests the token, and STS uses the Permissions Policy to generate temporary credentials (`AccessKeyId`, `Expiration`, `SecretAccessKey`, `SessionToken`). Credentials are returned to the identity requesting them. A trust policy determines who can assume a role.
