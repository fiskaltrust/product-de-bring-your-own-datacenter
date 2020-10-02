### fiskaltrust "Bring your own Datacenter"
# Quickstart

This document includes a step-by-step manual to run fiskaltrust ByoDC local on Docker Desktop for testing purposes.

### Requirements
- Docker Desktop (including Kubernetes activated)
![](images/ByoDC-Quickstart-0-DockerConfig.png)

## 1.) Install Helm
See [Helm documentation](https://helm.sh/docs/intro/install/)

## 2.) Create Kubernetes Namespace
```sh
kubectl create namespace bring-your-own-datacenter
```
![](images/ByoDC-Quickstart-1-Namespace.png)

## 3.) Add Helm Repo
```sh
helm repo add fiskaltrust https://charts.fiskaltrust.cloud/
```
![](images/ByoDC-Quickstart-2-AddRepo.png)

## 4.) Install HelmChart
```sh
helm install bring-your-own-datcenter fiskaltrust/bring-your-own-datacenter --namespace bring-your-own-datacenter
```
![](images/ByoDC-Quickstart-3-Install.png)

## 5.) Test in Browser
```sh
http://localhost/api/version
```
![](images/ByoDC-Quickstart-4-Browsertest.png)

## 7.) Cashbox and Queue Config
Create Cashbox and Queue Config (See [fiskaltrust Documentation](https://docs.fiskaltrust.cloud/doc/portal-manual-doc/doc/handbook-general/configuration.html))

![](images/ByoDC-Quickstart-5-CashboxConfig.png)

Insert your mysql Connectionstring into the Queue  
(f.i. Server=mysql;Port=3306;Uid=root;Pwd=password;)
![](images/ByoDC-Quickstart-6-QueueConfig.png)

After changing values inside the cashbox, don't forget to rebuild the cashbox configuration!

## 6.) Test with Postman
Start Postman and import the test [collection](https://github.com/fiskaltrust/product-de-bring-your-own-datacenter/blob/master/fiskaltrust%20DE%20ByoDC%20TestCall.postman_collection.json).

Edit the collection and set the cashbox_id and accesstoken from the fiskaltrust portal.
![](images/ByoDC-Quickstart-7-PostmanCollection.png)

Send a testrequest
![](images/ByoDC-Quickstart-8-PostmanResponse.png)
