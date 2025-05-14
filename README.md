<div style="text-align: center">
<img src="https://cdn.previder.io/vault/logo.png" width="250" alt=""/>
</div>

Vault-cli is a project to have a light-weight, secure and multi-tenant solution for encrypted password storage. Is uses the Vault Rest API where you can manage your environments, tokens and secrets.

**Release:**

[![Release Version](https://img.shields.io/github/v/release/previder/vault-cli?label=vault-cli)](https://github.com/previder/vault-cli/releases/latest)

**Last build:**

![Last build](https://github.com/previder/vault-cli/actions/workflows/go.yml/badge.svg)

**Last release:**

![Last publish](https://github.com/previder/vault-cli/actions/workflows/goreleaser.yml/badge.svg)

# Environments
Security is key in the project. You can create separate environments for your projects or customers. All environments use unique encryption keys, which are never stored in the database and are only available to the customer.

# Tokens
There are 3 types of tokens, each having its own purpose. The token received from the 

|                                     	 | EnvironmentAdmin  	 | ReadWrite  	 | ReadOnly   	 |
|----------------------------------|---------------------|--------------|--------------|
| Manage tokens                  	 | 	      ✅            | 	            | 	            |
| Manage secrets	                  | 	                   | 	  ✅          | 	            |
| Get decrypted secret             | 	                   | 	   ✅         | 	    ✅        |


The initial token received when a Secure Vault via the Previder Portal is created is of the type EnvironmentAdmin. This type of token can be used to manage ReadWrite or ReadOnly tokens, but not secrets.
An additional token of the type EnvironmentAdmin can also be created. Use the following command to create a token of type EnvironmentAdmin.

```shell
./vault-cli -t <insert-token> token create --description "EnvironmentAdmin token" --type EnvironmentAdmin
```

# Getting started
Vault-cli is a stand-alone binary to use with the Vault API. 

To see all usages, run
```shell
./vault-cli --help
```
The token can be used via the command-line itself.
```shell
./vault-cli -t <insert-token> secret list
```

To use a more secure method, set the token as an environment variable once to use it with the client.
```shell
export VAULT_TOKEN="insert-token"
./vault-cli secret list
```

# Creating tokens for secret management
## ReadWrite token
A ReadWrite type token can create, list, get, delete and decrypt secrets.
To create a ReadWrite token using the EnvironmentAdmin token, run the following command:
```shell
./vault-cli token create --description "ReadWrite token" --type ReadWrite
```

## ReadOnly token
A ReadOnly type token can only decrypt secrets of which an id or name are known. This type cannot manage secrets.
To create a ReadOnly token for use in a cluster, run the following command:

```shell
./vault-cli token create --description "ReadOnly token" --type ReadOnly
```

## Usage example
### List all tokens 
- Only available to EnvironmentAdmin type tokens
```shell
./vault-cli token list
```

### List all secrets
- Only available to ReadWrite type tokens
```shell
./vault-cli secret list
```

### Create a secret 
- Only available to ReadWrite type tokens
```shell
./vault-cli secret create --description "Example secret" --secret "SuperSecurePassword"
```

### Delete a secret
- Only available to ReadWrite type tokens
```shell
./vault-cli secret delete <id or description of the secret>
```

### Decrypt a secret
- Only available to ReadWrite and ReadOnly type tokens
```shell
./vault-cli secret decrypt <id or description of the secret>
```

To get the decrypted secret back to use in an application.

## Output
The default output format is `json`. Lists of environments, tokens and secrets can also be pretty-printed with the `-o pretty` parameter.
