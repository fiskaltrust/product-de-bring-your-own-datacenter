### fiskaltrust "Bring your own Datacenter"
# Parameter Reference

All settings in our helm-chart can be overruled by creating your own values.yaml.

The following settings are valid:

<br>

## Section "byodc"

| Subsection | Parameter                              |               Default Value               |                                                                                                               Description                                                                                                                |
| :--------- | :------------------------------------- | :---------------------------------------: | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| image      | name                                   |  ghcr.io/fiskaltrust/byodc-mysql-fiskaly  |                                                              URI of container image which is used as BackendPOD. This URI is preset to fiskaltrust github packges registry                                                               |
| image      | tag                                    |                1.3-buster                 |                                                                                                 override default version of the byod POD                                                                                                 |
| image      | pullSecret                             |                   false                   |                                                                         ByoDC is public available so the container registry can be usend without authentication                                                                          |
| config     | redis/host                             |                   redis                   |                                          Hostname for Redis instance (Must be DNS resolvable. By default the redis instance runs as POD on the same cluster so  cluster-resolution should work)                                          |
| proxy      | http                                   |                   null                    |                                                               HTTP_PROXY value according to https://docs.docker.com/network/proxy/#set-the-environment-variables-manually                                                                |
| proxy      | https                                  |                   null                    |                                                               HTTPS_PROXY value according to https://docs.docker.com/network/proxy/#set-the-environment-variables-manually                                                               |
| proxy      | no                                     |                   null                    |                                                                NO_PROXY value according to https://docs.docker.com/network/proxy/#set-the-environment-variables-manually                                                                 |
| proxy      | ftp                                    |                   null                    |                                                                FTP_PROXY value according to https://docs.docker.com/network/proxy/#set-the-environment-variables-manually                                                                |
| config     | redis/port                             |                   6379                    |                                                                                                      Port to access Redis instance                                                                                                       |
| config     | redis/channel                          |                   byodc                   |                                        Redis Pub/Sub channel which should be used. Take care to take 2 different channels if Producion and Sandbox environments are running on the same cluster!                                         |
| config     | applicationInsights/instrumentationkey |                   null                    |                                                                   Override Microsoft Application Insights Tenant. By default fiskaltrust Application Insights is used                                                                    |
| config     | helipad/baseurl                        | https://helipad-sandbox.fiskaltrust.cloud |                               URL for fiskaltrust.helipad to get Cashboxconfiguration and upload Data. (Sandbox: https://helipad-sandbox.fiskaltrust.cloud, Production: https://helipad.fiskaltrust.cloud)                               |
| config     | replicas                               |                    10                     |                                                         Number of BackendPODs which are deployed. See the "limits" section for calculation of needed noderesources and PODCount                                                          |
| config     | limits/cpu                             |                   500m                    |                                                  See Kubernetes doc [here](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/#resource-units-in-kubernetes)                                                  |
| config     | limits/memory                          |                  1000Mi                   |                                                                                                                                                                                                                                          |
| config     | timeout/all                            |                     0                     |   Emissary-ingress timeout for the complete TCP transaction in ms (0...no timeout. See [Emissary-ingress docs](https://www.getambassador.io/docs/emissary/latest/topics/using/timeouts/#request-timeout-timeout_ms) value "timeout_ms"   |
| config     | timeout/connect                        |                  15_000                   | Emissary-ingress timeout for the TCP connection esteblishment in ms. See [Emissary-ingress docs](https://www.getambassador.io/docs/emissary/latest/topics/using/timeouts/#connect-timeout-connect_timeout_ms) value "connect_timeout_ms" |
| config     | env                                    |                   null                    |                                      An Array of environment variables (see https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.22/#envvar-v1-core) which are added to the BackendPOD                                       |

<br><br>

## Section "fcc"

| Subsection | Parameter        |                       Default Value                        |                                                                          Description                                                                          |
| :--------- | :--------------- | :--------------------------------------------------------: | :-----------------------------------------------------------------------------------------------------------------------------------------------------------: |
| image      | name             | fscloudapps.azurecr.io/servicestack/fiskal-cloud-connector |                                                       URI of container image which is used as FCC POD.                                                        |
| image      | tag              |                           latest                           |                                                            override default version of the FCC POD                                                            |
| image      | pullSecret       |                           false                            |                                                                                                                                                               |
| proxy      | http             |                            null                            |                          HTTP_PROXY value according to https://docs.docker.com/network/proxy/#set-the-environment-variables-manually                          |
| proxy      | https            |                            null                            |                         HTTPS_PROXY value according to https://docs.docker.com/network/proxy/#set-the-environment-variables-manually                          |
| proxy      | no               |                            null                            |                           NO_PROXY value according to https://docs.docker.com/network/proxy/#set-the-environment-variables-manually                           |
| proxy      | ftp              |                            null                            |                          FTP_PROXY value according to https://docs.docker.com/network/proxy/#set-the-environment-variables-manually                           |
| config     | limits/cpu       |                           1000m                            |            See Kubernetes doc [here](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/#resource-units-in-kubernetes)             |
| config     | limits/memory    |                           1500Mi                           |                                                                                                                                                               |
| config     | storage/class    |                                                            |                                             Storage class to use for the PersistentVolumeClaim for the FCC volume                                             |
| config     | storage/capacity |                            1Gi                             |                                                   Capacity of the PersistentVolumeClaim for the FCC volume                                                    |
| config     | env              |                            null                            | An Array of environment variables (see https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.22/#envvar-v1-core) which are added to the BackendPOD |
| instances     |               |                            []                            | An Array of Fcc instances to host. Each entry needs to be a map with the following keys: `vtssId` and `vtssSecret` and optionaly `storageCapacity` and `storageClass`. see [here](how-to-fiscal-cloud-connector.md#ByoDC-configuration) |

<br><br>

## Section "redis"

| Subsection | Parameter | Default Value |                                                  Description                                                   |
| :--------- | :-------- | :-----------: | :------------------------------------------------------------------------------------------------------------: |
| enabled    |           |     true      | Deploy Redis POD for BackendPOD Syncronization (Not needed if you provide Redis outside the kubernetes custer) |

<br><br>

## Section "loadbalancer"

| Subsection | Parameter     | Default Value |                                                                                                                                                                                                 Description                                                                                                                                                                                                 |
| :--------- | :------------ | :-----------: | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| config     | tls/enabled   |     false     |                                                                                                                                                                                                                                                                                                                                                                                                             |
| config     | tls/crtBase64 |     null      | [see Emissary-ingress docs](https://www.getambassador.io/docs/emissary/latest/howtos/tls-termination/#create-a-self-signed-certificate) for information about `cert` and `key` files. You can use `--set-file ambassador.config.tls.crt="./cert.pem"` to pass files to the helm command. Please make sure the files start with `-----BEGIN [...]`. The cert file may also contain intermediate certificates |
| config     | tls/Keybase64 |     null      |                                                                                                                                                                                          Keyfile in Base64 String                                                                                                                                                                                           |
| config     | tls/crt       |     null      |                                                                                                                                                                                    CertFile content (string) not encoded                                                                                                                                                                                    |
| config     | tls/key       |     null      |                                                                                                                                                                                    KeyFile content (string) not encoded                                                                                                                                                                                     |
| config     | hostname      |       *       |                                                                                                                                                                                           HTTPS-Binding hostname                                                                                                                                                                                            |

## Section "emissary-ingress"

| Subsection   | Parameter       | Default Value |                               Description                                |
| :----------- | :-------------- | :-----------: | :----------------------------------------------------------------------: |
| install      |                 |     true      |    Install Emissary-ingress PODs for Loadbalancing as helm dependency    |
| replicaCount |                 |       2       |                         Count of redundant PODs                          |
| scope        | singleNamespace |     true      | If you want Emissary-ingress to only work outside of ByoDC, set to false |
