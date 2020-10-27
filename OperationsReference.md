### fiskaltrust "Bring your own Datacenter"
# Operations Reference

## Reset Cashbox via Echo Endpoint
Since several Cashboxes are hosted in one BackendPOD, there could be the requirement to restart only one particular Cashbox (f.i. to reload its config). To do so, the Echo Endpoint can be triggered.
```sh
/api/Echo

Body:
{
    "message": ""
}
```
By passing CashboxID and Accesstoken, the Cashbox does a immedeate reset and reload its config from helipad.fiskaltrust.cloud.

See our [Postmancollection](https://github.com/fiskaltrust/product-de-bring-your-own-datacenter/blob/master/fiskaltrust%20DE%20ByoDC%20TestCall.postman_collection.json) for an example call:
![](images/fiskaltrust-ByoDC-Echo-Postman.png)

This Endpoint can also be triggered several times sequential (e.g. in a bulk update scenario)

### Restart of all Cashboxes
In a massive update scenario (all or the vast majority of cashboxes were rebuilded) it is possible to restart all BackendPODs by triggering a rolling restart. K8S will send the SIGTERM Signal to the affected POD so current requests should be completed properly.

Example:
```
kubectl config set-context --current --namespace=bring-your-own-datacenter 
kubectl get deployments 
kubectl rollout restart deployment byodc
```


## LogFiles
fiskaltrust Middleware in ByoDC does not support local Logfiles.  
Usual logging practice in K8S environments is to handle logs via stdout/stderr.
https://kubernetes.io/docs/concepts/cluster-administration/logging/#cluster-level-logging-architectures



