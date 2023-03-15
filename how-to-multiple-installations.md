# Installing ByoDC multiple times on the same cluster

To do this you will also need to install Imissary Ingress multiple times directly into each of the byodc namespaces.

If you had previously installed Emissary Ingress on the cluster you may need to uninstall it before proceding.

## Emissary Ingress installation

To install Emissary Ingress multiple times you'll need to set some values in its configuration.

`emissary-config.yaml`:
```yaml
scope:
  singleNamespace: true

ingressClassResource:
  enabled: false

agent:
  enabled: false
```

You then need to install Emissary into all of the namespaces where you have byodc installations runnig:

```sh
helm install -n byodc1 emissary-ingress1 datawire/emissary-ingress -f emissary-config.yaml
helm install -n byodc2 emissary-ingress2 datawire/emissary-ingress -f emissary-config.yaml
```