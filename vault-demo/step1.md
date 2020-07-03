Installation

## Task

### Download package from Hashicorp site - v1.4.3
Unzipped package contains executable file - place it anywhere in the PATH (eg. /usr/bin)
```
wget https://releases.hashicorp.com/vault/1.4.3/vault_1.4.3_linux_amd64.zip
unzip vault_1.4.3_linux_amd64.zip
cp ./vault /usr/bin/vault
```{{execute}}

### Download and work on a docker image - latest
(remember to enter the container in every terminal opened in the following steps - otherwise vault will not work)
`docker run -it vault:latest ash`{{execute}}

### Download using snap (old v1.1.1 - not recommended)
`snap install vault`{{execute}}

## Verify installation
`vault -v`{{execute}}
