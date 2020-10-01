### fiskaltrust "Bring your own Datacenter"
# Parameter Reference

All settings in our helm-chart can be overruled by creating your own values.yaml.

The following settings are valid:

<br>

## Section "byodc"

| Subsection | Parameter | Default Value | Description |
| :----- | :----- | :------: | :-----------: |
| image | name | docker.pkg.github.com/fiskaltrust/product-de-bring-your-own-datacenter/byodc-mysql-fiskaly | URI of container image which is used as BackendPOD. This URI is preset to fiskaltrust github packges registry|
| image | tag | null | override default version of the byod POD |
| image | pullsecret | false | ByoDC is public available so the container registry can be usend without authentication |
| config | redis/host | redis | Hostname for Redis instance (Must be DNS resolvable. By default the redis instance runs as POD on the same cluster so  cluster-resolution should work) |
| config | redis/port | 6379 | Port to access Redis instance |
| config | redis/channel | byodc | Redis Pub/Sub channel which should be used. Take care to take 2 different channels if Producion and Sandbox environments are running on the same cluster! |
| config | applicationInsights/instrumentationkey | null | Override Microsoft Application Insights Tenant. By default fiskaltrust Application Insights is used |
| config | helipad/baseurl | https://helipad-sandbox.fiskaltrust.cloud | URL for fiskaltrust.helipad to get Cashboxconfiguration and upload Data. (Sandbox: https://helipad-sandbox.fiskaltrust.cloud, Production: https://helipad.fiskaltrust.cloud) |
| config | replicas | 10 | Number of BackendPODs which are deployed. See the "limits" section for calculation of needed noderesources and PODCount |
| config | limits/cpu | 100m | See Kubernetes doc [here](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/#resource-units-in-kubernetes)  
| config | limits/memory | 100Mi |  |
| config | timeout/all | 0 | Ambassador timeout for the complete TCP transaction in ms (0...no timeout. See [Ambassador doc](https://www.getambassador.io/docs/latest/topics/using/timeouts/#request-timeout-timeout_ms) value "timeout_ms"|
| config | timeout/connect | 15_000 | Ambassador timeout for the TCP connection esteblishment in ms. See [Ambassador doc](https://www.getambassador.io/docs/latest/topics/using/timeouts/#connect-timeout-connect_timeout_ms) value "connect_timeout_ms" |
|  |  |  |  |

<br><br>

## Section "redis"

| Subsection | Parameter | Default Value | Description |
| :----- | :----- | :------: | :-----------: |
| enabled |  | true | Deploy Redis POD for BackendPOD Syncronization (Not needed if you provide Redis outside the kubernetes custer) |

<br><br>

## Section "ambassador"

| Subsection | Parameter | Default Value | Description |
| :----- | :----- | :------: | :-----------: |
| install |  | true | Install Ambassador PODs for Loadbalancing as helm dependency|
| config | tls/enabled | false |  |
| config | tls/crtBase64 | null | [see Ambassador Doc](https://www.getambassador.io/docs/latest/howtos/tls-termination/#create-a-self-signed-certificate) for information about `cert` and `key` files. You can use `--set-file ambassador.config.tls.crt="./cert.pem"` to pass files to the helm command. Please make sure the files start with `-----BEGIN [...]`. The cert file may also contain intermediate certificates |
| config | tls/Keybase64 | null | Keyfile in Base64 String |
| config | tls/crt | null | CertFile content (string) not encoded |
| config | tls/key | null | KeyFile content (string) not encoded |
| config | hostname | * | HTTPS-Binding hostname |
| replicaCount |  | 2 | Count of redundant PODs |

