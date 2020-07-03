Production config

Vault is configured using HCL (HashiCorp Configuration Language) files.
Configuration may be also passed as JSON.

This object in HCL
```
variable "ami" {
    description = "the AMI to use"
}
```
is equal to this object in JSON
```
{
  "variable": {
      "ami": {
          "description": "the AMI to use"
        }
    }
}
```

## Task


Let's start the server with the following configuration:

```
storage "file" {
  path    = "/root/storage"
}

listener "tcp" {
 address     = "127.0.0.1:8200"
 tls_disable = 1
}
```

storage - This is the physical backend that Vault uses for storage. Up to this point the dev server has used "inmem" (in memory), but the example above uses file as a storage.

listener - One or more listeners determine how Vault listens for API requests. The example above listens on localhost port 8200 without TLS. In your environment set VAULT_ADDR=http://127.0.0.1:8200 so the Vault client will connect without TLS.


First, stop the dev server on Terminal 1 using <kbd>Ctrl</kbd>+<kbd>C</kbd> keyboard shortcut.

Then, create a file with the configuration mentioned above
`echo -e "storage \"file\" {\n  path = \"/root/storage\"\n}\nlistener \"tcp\" {\n  address = \"0.0.0.0:8200\"\n  tls_disable = 1\n}" > config.hcl`{{execute}}

To start the server, use the following command
`vault server -config=config.hcl`{{execute}}
