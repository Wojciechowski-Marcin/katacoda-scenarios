First secret

## Task

### Writing secrets

Writing a secret is done with `vault kv put` command. kv stands for key-value engine.

`vault kv put secret/hello foo=world`{{execute T2}}

It writes the pair foo=world to the path secret/hello.

For now it is important that the path is prefixed with `secret/`

You can also write multiple pieces of data at once. Let's put some more data into another path `secret/world`.

`vault kv put secret/world bar=foo excited=yes`{{execute T2}}

### Getting secrets

Getting the data is done with `vault kv get`

`vault kv get secret/world`{{execute T2}}

Vault gets the data from storage and decrypts it.

### Deleting secrets

Deleting the data is done with (what a surprise) `vault kv delete` command.

`vault kv delete secret/world`{{execute T2}}
