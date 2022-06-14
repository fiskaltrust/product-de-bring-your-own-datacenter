# Single-instance installations
In scenarios where only a very small number of users need to connect to BYODC, and where high-availability is not a crucial factor, it's possible to run a single instance of the BYODC container. This may be especially beneficial in cases where no K8s environment is running yet, but a cloud provider is used that offers Docker-based image hosting (e.g. _Azure Container Instances_ or _Amazon ECS_).

> **Warning**
> 
> Please note that this scenario introduces a _single point of failure_, as there are no redudancies in place. We recommend to use the full Kuberentes depoloyment in scenarios that require high-availability.

## Setup guide
The exact steps of setting up BYODC in a single container depend on the respective hosting provider. However, the important key facts are always similar:
1. The **Docker image** is hosted on our public [GH container registry](https://github.com/fiskaltrust/product-de-bring-your-own-datacenter/pkgs/container/byodc), the latest version can be obtained as `ghcr.io/fiskaltrust/byodc:latest`.
2. The web application within the container uses **port 80**. Depending on your setup, this port should be either directly published or aliased.
3. There are two environment variables that need to be set:
   - `Helipad__BaseUrl`: Either `https://helipad.fiskaltrust.cloud` (_for production_) or `https://helipad-sandbox.fiskaltrust.cloud` (_for sandbox_) must be used
   - `Redis__ConnectionString`: Must be set to an empty string to signalize that no Redis synchronization is used.
4. We strongly recommend to use a TLS certificate, which most cloud providers natively support for hosted containers.

## Local testing
The BYODC container can also be tested locally via Docker:
```bash
docker run -d -e Helipad__BaseUrl='https://helipad-sandbox.fiskaltrust.cloud' -e Redis__ConnectionString='' -p 8080:80 ghcr.io/fiskaltrust/byodc:latest
```
