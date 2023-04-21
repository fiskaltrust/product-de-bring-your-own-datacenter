### fiskaltrust "Bring your own Datacenter"
# QuickStart

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

## 3.) Install Emissary Ingress

Install Emissary Ingress v3.x using the [official installation instructions](https://www.getambassador.io/docs/emissary/3.5/topics/install/helm).

## 4.) Add Helm Repo
```sh
helm repo add fiskaltrust https://charts.fiskaltrust.cloud/
```
![](images/ByoDC-Quickstart-2-AddRepo.png)

## 5.) Install HelmChart

### Option 1 (existing MySQL Server needed)

```sh
helm install bring-your-own-datcenter fiskaltrust/bring-your-own-datacenter --namespace bring-your-own-datacenter
```

> ***Note:** If you use a local repo you will have to run `helm dependency update` before installing.*

![](images/ByoDC-Quickstart-3-Install.png)

### Option 2 (including MySQL POD)

If you would like to have a mysql container for easy testing, you can add a config setting before installing the HelmChart as follows:

1.  Create a ```config.yaml``` file and add following content:
```yaml
mysql:
  enabled: true
```
2. Pass the created  ```config.yaml``` file with the helm install command:

```sh
helm install bring-your-own-datcenter fiskaltrust/bring-your-own-datacenter --namespace bring-your-own-datacenter -f config.yaml
```
This will additionally create a mysql container that you can use in Step 7.) when configuring the Queue for testing. 

>**Attention:** Be aware that it is **not recommended** to use a MySQL single instance POD for production environments. This Setting is not included in the parameter reference and only for testing scenarios!

## 6.) Test reachability in the Browser

Before testing reachability in the browser please wait a couple of minutes because the first time Kubernetes has to pull the Docker Image, this may take a while.

```sh
http://localhost/api/version
```
![](images/ByoDC-Quickstart-4-Browsertest.png)

## 7.) Create a Cashbox in the [Sandbox ft.Portal](https://portal-sandbox.fiskaltrust.de/CashBox)

7.1) First create a new Queue

- choose the Package ```fiskaltrust.Middleware.Queue.MySQL```

- insert a value for the ```CashboxIdentification``` field - this will be used as the POS System serial number (Kassenseriennummer) and as a client id in the TSE. It must be a [printable string](https://en.wikipedia.org/wiki/PrintableString) with a maximum length of 20 characters.

- press "Save" and in the next form insert your mysql Connectionstring:  
(e.g. ```Server=mysql;Port=3306;Uid=root;Pwd=password;``` for Option 1 - MySQL POD)

- add a http(REST) endpoint by clicking the corresponding button

  ![](images/ByoDC-Quickstart-7-QueueConfig.png)

- Save and close

7.2 ) Next create a new SCU 

- for testing you can use a fiskaly TSE.  To obtain the TSE access data, register in the [fiskaly dashboard](https://dashboard.fiskaly.com/) and create there a test TSE. 
- create the new SCU in the portal and choose the Package ```fiskaltrust.Middleware.SCU.DE.Fiskaly```
- press "Save" and next enter the TSE credentials received from the fiskaly dashboard.
- create a grpc endpoint by pressing the corresponding button. 
- Save and close.

7.3) Create a new Cashbox
- enter a name an press the "Save" button. 
- in the list of cashboxes press the "edit by list" button in the row of the newly created cashbox 
- in the appearing screen select the newly created Queue and SCU to add them to the new cashbox and press "Save"
- back in the list of cashboxes expand the row of the newly created cashbox and press the first button right to the Queue name (arrow right up). A popup appears with a list of SCUs. Select the newly created SCU to connect the Queue with the SCU.
- in the same list of cashboxes press the "Rebuild configuration" button in the row of the newly created cashbox.
- expand this row to see the credentials of the cashbox (```CashBoxId``` and ```Access token```)

![](images/ByoDC-Quickstart-5-CashboxConfig.png)

After changing values inside the cashbox (including Queue and SCU), don't forget to rebuild the cashbox configuration!

## 8.) Test with Postman

8.1) Start our ByoDC test collection directly from here: [![Run in Postman](https://run.pstmn.io/button.svg)](https://app.getpostman.com/run-collection/f25ba9a9c934a6e997ec)

 or start Postman, download and import the [ByoDC test collection](https://github.com/fiskaltrust/product-de-bring-your-own-datacenter/blob/master/fiskaltrust%20DE%20ByoDC%20TestCall.postman_collection.json).

8.2) Edit the collection and set the current value for the variables cashbox_id and accesstoken with the credentials from the fiskaltrust portal (see step 6.3). This will automatically add them to the headers of the requests.

![](images/ByoDC-Quickstart-7-PostmanCollection.png)



8.3) Send the echo request to see if the queue is reachable. The echo request can also be used to relaod an updated configuration (after pressing the rebuild configuration button in the portal).

![](images/ByoDC-Quickstart-7-3-Echo.png)



8.4) Send the initial operation request to register the cashbox as a client in the TSE. This will also initialize the TSE if it is not yet initialized.

![](images/ByoDC-Quickstart-7-4-Initial-Operation-Receipt.png)



8.5) Send the pos-receipt (ByoDC Barumsatz).#
![](images/ByoDC-Quickstart-7-5-POS-Receipt.png)



8.6) Request the TSE Info by sending the flagged zero receipt.

![](images/ByoDC-Quickstart-7-6-TSE-Info.png)

## 9.) Hints

If you later need to delete the namespace you can do it with following command:

```sh
kubectl delete namespace bring-your-own-datacenter
```

