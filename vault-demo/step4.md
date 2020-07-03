Secrets Engines

## Task

In the previous part it was important that the secret path was prefixed with `secret/`, because thats the engine created by default in vault.

### Creating secrets engine

To create your own, you need to use `vault secrets enable` command

`vault secrets enable -path=kv kv`{{execute T2}}

This creates `kv` secret engine at the path `kv/`
If the path is equal to the name, `-path` parameter can be skipped. In our example the command above would be equal to the command below:

`vault secrets enable kv`{{execute T2}}

To see all secrets engines, use this command:

`vault secrets list`{{execute T2}}

### Testing the new secrets engine

Let's put some data into our engine.

`vault kv put kv/hello target=world`{{execute T2}}

And let's check if it's working properly.

`vault kv get kv/hello`{{execute T2}}

### Disabling the secrets engine

When a secret is no longer needed, it can be disabled. When an engine is disabled, all secrets and corresponding vault data and configuration is removed.

`vault secrets disable kv/`{{execute T2}}
