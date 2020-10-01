### fiskaltrust "Bring your own Datacenter"
# HTTPS Requirements
## Create `pem` files

```sh
openssl pkcs12 -in cert.pfx -nokeys -out crt.pem -nodes
openssl pkcs12 -in cert.pfx -nocerts -out key.pem -nodes
```

> ***Note:** The file `crt.pem` can contain the whole certificate chain.*

The files should look like this:

> `crt.pem`
```
-----BEGIN CERTIFICATE-----
MIIGRTCCBS2gAwIBAgIQBnq/+1Pnn7lVuiljZmvbRDANBgkqhkiG9w0BAQsFADBN
................................................................
................................................................
MrIw+XVhEiTdDlnXwKVJgf3LcqN+6Ntk3w==
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
MIIDrzCCApegAwIBAgIQCDvgVpBCRrGhdWrJWZHHSjANBgkqhkiG9w0BAQUFADBh
................................................................
................................................................
CAUw7C29C79Fv1C5qfPrmAESrciIxpg0X40KPMbp1ZWVbd4=
-----END CERTIFICATE-----

-----BEGIN CERTIFICATE-----
MIIElDCCA3ygAwIBAgIQAf2j627KdciIQ4tyS8+8kTANBgkqhkiG9w0BAQsFADBh
................................................................
j6tJLp07kzQoH3jOlOrHvdPJbRzeXDLz
-----END CERTIFICATE-----
```

> `key.pem` 
```
-----BEGIN PRIVATE KEY-----
MIIEvgIBADANBgkqhkiG9w0BAQEFAASCBKgwggSkAgEAAoIBAQDW1k6gbMgR0+Zs
................................................................
................................................................
5CAuLEmm4KZrnKwR4IPt2X4U
-----END PRIVATE KEY-----
```

If there is any text (e.g. Bag Attributes) outside of the `-----BEGIN ...-----` and `-----END ...-----` sections make sure to remove it.

## Setup config

You need to enable tls in the `config.yaml`.
```yaml
ambassador: 
  config: 
    tls:
      enabled: true
```

## Deploy
You also need to provide the `crt.pem` and `key.pem` files when deploying.

You can either provide directly them in the `config.yaml` or as command line arguments.

### `config.yaml`

You can either provide the file contents as a base64 coded string:
```yaml
ambassador: 
  config: 
    tls:
      enabled: true
      crtBase64: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLQouCi4KLgotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCgotLS0tLUJFR0lOIENFUlRJRklDQVRFLS0tCi4KLgouCi0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0KCi0tLS0tQkVHSU4gQ0VSVElGSUNBVEUtLS0KLgouCi4KLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLQ==
      keyBase64: LS0tLS1CRUdJTiBSU0EgUFJJVkFURSBLRVktLS0tLQouCi4KLgotLS0tLUVORCBSU0EgUFJJVkFURSBLRVktLS0tLQ==
```

Or you can provide the file contents directly:
```yaml
ambassador: 
  config: 
    tls:
      enabled: true
      crt: |
        -----BEGIN CERTIFICATE---
        .
        .
        .
        -----END CERTIFICATE-----

        -----BEGIN CERTIFICATE---
        .
        .
        .
        -----END CERTIFICATE-----

        -----BEGIN CERTIFICATE---
        .
        .
        .
        -----END CERTIFICATE-----
      key: |
        -----BEGIN RSA PRIVATE KEY-----
        .
        .
        .
        -----END RSA PRIVATE KEY-----
```

### command line arguments

If you don't want the certificates stored in the config you can provide them when deploying.

You can either provide the file contents as a base64 coded string using the `--set` parameter:
```sh
helm install bring-your-own-datcenter fiskaltrust/bring-your-own-datacenter --namespace bring-your-own-datacenter \
  -f config.yaml \
  --set ambassador.config.tls.crtBase64="LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLQouCi4KLgotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCgotLS0tLUJFR0lOIENFUlRJRklDQVRFLS0tCi4KLgouCi0tLS0tRU5EIENFUlRJRklDQVRFLS0tLS0KCi0tLS0tQkVHSU4gQ0VSVElGSUNBVEUtLS0KLgouCi4KLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLQ==" \
  --set ambassador.config.tls.keyBase64="LS0tLS1CRUdJTiBSU0EgUFJJVkFURSBLRVktLS0tLQouCi4KLgotLS0tLUVORCBSU0EgUFJJVkFURSBLRVktLS0tLQ=="

```

Or you can provide the file path directly using the `--set-file` parameter:
```sh
helm install bring-your-own-datcenter fiskaltrust/bring-your-own-datacenter --namespace bring-your-own-datacenter \
  -f config.yaml \
  --set-file ambassador.config.tls.crt="./crt.pem" \
  --set-file ambassador.config.tls.key="./key.pem"

```
