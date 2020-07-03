Starting server

## Task

### Starting dev server

Development server is built in, let us try starting it

`vault server -dev`{{execute}}

You will see server logs, but the important part is placed above:

```
WARNING! dev mode is enabled! In this mode, Vault runs entirely in-memory
and starts unsealed with a single unseal key. The root token is already
authenticated to the CLI, so you can immediately begin using Vault.

You may need to set the following environment variable:

    $ export VAULT_ADDR='http://127.0.0.1:8200'

The unseal key and root token are displayed below in case you want to
seal/unseal the Vault or re-authenticate.

Unseal Key: T0bfP0JXebLBh5qklWL0p8tOe+kre5EXNQGVpa02d+o=
Root Token: s.snaXndd5pV4cbR65NHw1kkjU

Development mode should NOT be used in production installations!
```

Notice that Unseal Key and Root Token values are displayed.

### Setting essential env variables

With the dev server started, launch a new terminal session and export the following variables:

`export VAULT_ADDR='http://127.0.0.1:8200'`{{execute T2}}
`export VAULT_DEV_ROOT_TOKEN_ID='{YOUR_ROOT_TOKEN}'`{{copy}}

With these variables, we will be able to authenticate with Vault later.

### Verify the running server

`vault status`{{execute T2}}
