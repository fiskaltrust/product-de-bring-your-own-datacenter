### fiskaltrust "Bring your own Datacenter"
# Parameter Reference

All settings in our helm-chart can be overruled by creating your own values.yaml.

The following settings are valid:

<br>

## Section "byodc"

| Subsection | Parameter                              |               Default Value               |                                                                                                               Description                                                                                                                |
| :--------- | :------------------------------------- | :---------------------------------------: | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| image      | name                                   |  ghcr.io/fiskaltrust/byodc-mysql-fiskaly  |                                                              URI of container image which is used as BackendPOD. This URI is preset to fiskaltrust github packges registry                                                               |
| image      | tag                                    |                1.3-buster                 |                                                                                                override default version of the ByoDC POD                                                                                                 |
| image      | pullSecret                             |                   false                   |                                                                         ByoDC is public available so the container registry can be usend without authentication                                                                          |
| config     | redis/host                             |                   redis                   |                                          Hostname for Redis instance (Must be DNS resolvable. By default the redis instance runs as POD on the same cluster so  cluster-resolution should work)                                          |
| proxy      |                                        |                   null                    |                                      Proxy string according to https://docs.fiskaltrust.cloud/docs/posdealers/technical-operations/middleware/network-requirements#setting-the-proxy-configuration                                       |
| config     | redis/port                             |                   6379                    |                                                                                                      Port to access Redis instance                                                                                                       |
| config     | redis/channel                          |                   byodc                   |                                        Redis Pub/Sub channel which should be used. Take care to take 2 different channels if Producion and Sandbox environments are running on the same cluster!                                         |
| config     | applicationInsights/instrumentationkey |                   null                    |                                                                   Override Microsoft Application Insights Tenant. By default fiskaltrust Application Insights is used                                                                    |
| config     | helipad/baseurl                        | https://helipad-sandbox.fiskaltrust.cloud |                               URL for fiskaltrust.helipad to get Cashboxconfiguration and upload Data. (Sandbox: https://helipad-sandbox.fiskaltrust.cloud, Production: https://helipad.fiskaltrust.cloud)                               |
| config     | replicas                               |                    10                     |                                                         Number of BackendPODs which are deployed. See the "limits" section for calculation of needed noderesources and PODCount                                                          |
| config     | limits/cpu                             |                   500m                    |                                                  See Kubernetes doc [here](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/#resource-units-in-kubernetes)                                                  |
| config     | limits/memory                          |                  1000Mi                   |                                                                                                                                                                                                                                          |
| config     | logLevel                               |                Information                |                                                  See [ASP.Net log levels](https://learn.microsoft.com/en-us/dotnet/api/microsoft.extensions.logging.loglevel?view=dotnet-plat-ext-7.0)                                                   |
| config     | timeout/all                            |                     0                     |   Emissary-ingress timeout for the complete TCP transaction in ms (0...no timeout. See [Emissary-ingress docs](https://www.getambassador.io/docs/emissary/latest/topics/using/timeouts/#request-timeout-timeout_ms) value "timeout_ms"   |
| config     | timeout/connect                        |                  15_000                   | Emissary-ingress timeout for the TCP connection esteblishment in ms. See [Emissary-ingress docs](https://www.getambassador.io/docs/emissary/latest/topics/using/timeouts/#connect-timeout-connect_timeout_ms) value "connect_timeout_ms" |
| config     | env                                    |                   null                    |                                      An Array of environment variables (see https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.22/#envvar-v1-core) which are added to the BackendPOD                                       |
| config     | nodeSelector                           |                    {}                     |                                                 Sets the `nodeSelector` property of the ByoDC PODs https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#nodeselector                                                 |

<br><br>

## Section "fcc"

| Subsection | Parameter        |                       Default Value                        |                                                                                                               Description                                                                                                               |
| :--------- | :--------------- | :--------------------------------------------------------: | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| image      | name             | fscloudapps.azurecr.io/servicestack/fiskal-cloud-connector |                                                                                            URI of container image which is used as FCC POD.                                                                                             |
| image      | tag              |                           latest                           |                                                                                                 override default version of the FCC POD                                                                                                 |
| image      | pullSecret       |                           false                            |                                                                                                                                                                                                                                         |
| config     | limits/cpu       |                           1000m                            |                                                 See Kubernetes doc [here](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/#resource-units-in-kubernetes)                                                  |
| config     | limits/memory    |                           1500Mi                           |                                                                                                                                                                                                                                         |
| config     | storage/class    |                                                            |                                                                                  Storage class to use for the PersistentVolumeClaim for the FCC volume                                                                                  |
| config     | storage/capacity |                            1Gi                             |                                                                                        Capacity of the PersistentVolumeClaim for the FCC volume                                                                                         |
| config     | env              |                            null                            |                                      An Array of environment variables (see https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.22/#envvar-v1-core) which are added to the BackendPOD                                      |
| config     | nodeSelector     |                             {}                             |                                                 Sets the `nodeSelector` property of the FCC PODs https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#nodeselector                                                  |
| instances  |                  |                             []                             | An Array of Fcc instances to host. Each entry needs to be a map with the following keys: `vtssId` and `vtssSecret` and optionaly `storageCapacity` and `storageClass`. see [here](how-to-fiscal-cloud-connector.md#ByoDC-configuration) |

<br><br>

## Section "redis"

| Subsection | Parameter    | Default Value |                                                               Description                                                               |
| :--------- | :----------- | :-----------: | :-------------------------------------------------------------------------------------------------------------------------------------: |
| enabled    |              |     true      |             Deploy Redis POD for BackendPOD Syncronization (Not needed if you provide Redis outside the kubernetes custer)              |
| config     | nodeSelector |      {}       | Sets the `nodeSelector` property of the redis POD https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#nodeselector |

<br><br>

## Section "loadbalancer"

| Subsection | Parameter     | Default Value |                                                                                                                                                                                                 Description                                                                                                                                                                                                 |
| :--------- | :------------ | :-----------: | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| config     | tls/crtBase64 |     null      | [see Emissary-ingress docs](https://www.getambassador.io/docs/emissary/latest/howtos/tls-termination/#create-a-self-signed-certificate) for information about `cert` and `key` files. You can use `--set-file ambassador.config.tls.crt="./cert.pem"` to pass files to the helm command. Please make sure the files start with `-----BEGIN [...]`. The cert file may also contain intermediate certificates |
| config     | tls/Keybase64 |     null      |                                                                                                                                                                                          Keyfile in Base64 String                                                                                                                                                                                           |
| config     | tls/crt       |     null      |                                                                                                                                                                                    CertFile content (string) not encoded                                                                                                                                                                                    |
| config     | tls/key       |     null      |                                                                                                                                                                                    KeyFile content (string) not encoded                                                                                                                                                                                     |

## Section "emissary"

| Subsection | Parameter | Default Value |                                   Description                                         |
| :--------- | :-------- | :-----------: | :-----------------------------------------------------------------------------------: |
| use        |           |     true      |                     Deploys the emissary ingress custom resources                     |
| tls        |           |     false     | If you want Emissary-ingress to use tls (cert configured in the loadbalancer section) |
| hostname   |           |      *        |       If you want Emissary-ingress to only work outside of ByoDC, set to false        |
