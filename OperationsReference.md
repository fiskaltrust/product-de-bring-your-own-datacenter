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