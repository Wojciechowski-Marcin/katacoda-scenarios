API

## Task

### Initialize API

We can initialize Vault API for all interactions like this (`jq` is used to process JSON output for readability)

```
curl \
  --request POST \
  --data '{ "secret_shares": 1, "secret_threshold": 1 }' \
  http://127.0.0.1:8200/v1/sys/init | jq
```{{execute T2}}

The response will be something like this:
```
{
  "keys": [
    "ff27b63de46b77faabba1f4fa6ef44c948e4d6f2ea21f960d6aab0eb0f4e1391"
  ],
  "keys_base64": [
    "/ye2PeRrd/qruh9Ppu9EyUjk1vLqIflg1qqw6w9OE5E="
  ],
  "root_token": "s.Ga5jyNq6kNfRMVQk2LY1j9iu"
}
```
root_token is out initial root token. Export this variable to the environment, as we will use it often.

`VAULT_TOKEN="root_token"`{{copy}}

keys_base64 is the unseal key.

### Seal

Seal is an additional protection for the service. On the development server there's no need to unseal the server, because root token works as a master key. On production server, this is necessary.

Let's try to get data from the storage using this endpoint:
```
curl \
  --header "X-Vault-Token: $VAULT_TOKEN" \
  http://127.0.0.1:8200/v1/secret/data/foo | jq
```{{execute T2}}

The server will tell us that we can't get such information, because the vault is sealed.
```
{
  "errors": [
    "error performing token check: Vault is sealed"
  ]
}
```

Unseal the vault like this:
```
curl \
  --request POST \
  --data '{ "key": "{keys_base64}" }' http://127.0.0.1:8200/v1/sys/unseal | jq
```{{copy}}

Where {keys_base64} should be replaced with the variable that we received after the init command.
The Vault is unsealed now - the operation will be logged in the terminal 1.

### ACL Policies

Policies are deny by default, so an empty policy grants no permission in the system.
That means, that to be able to work on secrets in Vault, we need to create our own policy.

First, let's enable version 2 of the kv engine
```
curl \
  --header "X-Vault-Token: $VAULT_TOKEN" \
  --request POST \
  --data '{ "type": "kv-v2" }' \
  http://127.0.0.1:8200/v1/sys/mounts/secret
```{{execute T2}}
Because of that, instead of creating secrets at path eg. secret/foo, the path looks like this: secret/data/foo.

To create the policy, use the following command:
```
curl \
  --header "X-Vault-Token: $VAULT_TOKEN" \
  --request PUT \
  --data '{ "policy": "path \"secret/data/*\" {\n  capabilities = [\"create\", \"update\"]\n}\n\npath \"secret/data/foo\" {\n  capabilities = [\"read\"]\n}\n"}' \
  http://127.0.0.1:8200/v1/sys/policies/acl/my-policy
```{{execute T2}}

We made a policy, that allows to create and update secrets at all children of a secret/data/ path and read secrets at secret/data/foo path.

### Creating and getting data

Finally we have enough knowledge to work on secrets through API.

Let's create the data at the foo endpoint

```
curl \
  --request POST \
  --data '{ "data": { "hello": "world" } }' \
  --header "X-Vault-Token: $VAULT_TOKEN" \
  http://127.0.0.1:8200/v1/secret/data/foo | jq
```{{execute T2}}

And to confirm that everything works correctly, get the data from the Vault.
```
curl \
  --header "X-Vault-Token: $VAULT_TOKEN" \
  http://127.0.0.1:8200/v1/secret/data/foo | jq
```{{execute T2}}
