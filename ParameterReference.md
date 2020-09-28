### fiskaltrust "Bring your own Datacenter"
# Parameter Reference

All settings in our helm-chart can be overruled by creating your own values.yaml.

The following settings are valid:

## Section "signaturcloud"

| Subsection | Parameter | Default Value | Description |
| :----- | :----- | :------: | :-----------: |
| image | name | docker.pkg.github.com/fiskaltrust/product-de-bring-your-own-datacenter/signaturecloud-mysql-fiskaly | URI of container image which is used as BackendPOD. This URI is preset to fiskaltrust github packges registry|
| image | tag | latest | Version of BackendPOD to use (latest is recommended) |
| image | pullsecret | false | ByoDC is public available so the container registry can be usend without authentication |
| config | redis/host | redis | Hostname for Redis instance (Must be DNS resolvable. By default the redis instance runs as POD on the same cluster so  cluster-resolution should work) |
| config | redis/port | 6379 | Port to access Redis instance |
| config | redis/channel | signaturecloud_dev | Redis Pub/Sub channel which should be used. Take care to take 2 different channels if Producion and Sandbox environments are running on the same cluster! |
| config | applicationInsights/instrumentationkey | '' | Override Microsoft Application Insights Tenant. By default fiskaltrust Application Insights is used |
| config | helipad/baseurl | https://helipad-sandbox.fiskaltrust.cloud | URL for fiskaltrust.helipad to get Cashboxconfiguration and upload Data. (Sandbox: https://helipad-sandbox.fiskaltrust.cloud, Production: https://helipad.fiskaltrust.cloud) |
| config | replicas | 10 | Number of BackendPODs which are deployed. See the "limits" section for calculation of needed noderesources and PODCount |
| config | limits/cpu | 100m |  |
| config | limits/memory | 100Mi |  |
| config | prefix | / |  |
| config | timeout/all | 0 |  |
| config | timeout/connect | 15_000 |  |
|  |  |  |  |
