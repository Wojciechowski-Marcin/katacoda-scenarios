Authentication

## Task

Token authentication is automatically enabled. The Vault CLI reads the root token from `$VAULT_DEV_ROOT_TOKEN_ID` environment variable. This can perform any operation within Vault because it is assigned the `root` policy.

Create a new token:
`vault token create`{{execute T2}}

You should see a similar output:
```
$ vault token create
Key                  Value
---                  -----
token                s.iyNUhq8Ov4hIAx6snw5mB2nL
token_accessor       maMfHsZfwLB6fi18Zenj3qh6
token_duration       ∞
token_renewable      false
token_policies       ["root"]
identity_policies    []
policies             ["root"]
```

This token is a child of the root token, and by default, it inherits the policies from its parent.

Login with this new token
`vault login {NEW_TOKEN}`{{copy}}

```
$ vault login s.iyNUhq8Ov4hIAx6snw5mB2nL

Success! You are now authenticated. The token information displayed below
is already stored in the token helper. You do NOT need to run "vault login"
again. Future Vault requests will automatically use this token.

Key                  Value
---                  -----
token                s.iyNUhq8Ov4hIAx6snw5mB2nL
token_accessor       maMfHsZfwLB6fi18Zenj3qh6
token_duration       ∞
token_renewable      false
token_policies       ["root"]
identity_policies    []
policies             ["root"]
```

### Revoking tokens

Create another token
`vault token create'`{{execute T2}}

Revoke the first token created
`vault token revoke {FIRST_TOKEN}`{{copy}}
Sample output:
```
$ vault token revoke s.iyNUhq8Ov4hIAx6snw5mB2nL

Success! Revoked token (if it existed)
```

Attempt to login with the last token you created
`vault login {NEW_TOKEN}`{{copy}}

You should see the following output:
```
$ vault login s.TsKT5ubouZ7TF26Eg7wNIl3k
Error authenticating: error looking up token: Error making API request.

URL: GET http://127.0.0.1:8200/v1/auth/token/lookup-self
Code: 403. Errors:

* permission denied
```

That is because revoking a token will revoke all the tokens that it created.

Login with the root token again
`vault login $VAULT_DEV_ROOT_TOKEN_ID`{{execute T2}}
