# Host FiscalCloudConnector instances in Bring your own Data Center

It is possible to host a FiscalCloudConnector (FCC) as a docker container. ByoCD can be configured to host preconfigured FCC.

## ByoDC Configuration

The helm chart needs to be provided with the FccId (`vtssId`) and FccSecret (`vtssSecret`) of each FCC.

To do that add the following to the `config.yaml` file:

```yaml
fcc:
  instances:
    - vtssId: <FccId>
      vtssSecret: <FccSecret>
```

Each FCC gets assigned a volume containing the `.fccdata`. This volume is created using a `PersistentVolumeClaim`.

Per default the default storage class of your cluster and a capacity of 1GB is used.\
This can be overriden like so:

```yaml
fcc:
  config:
    storage:
      class: <defaultStorageClass>
      capacity: <defaultCapacity>
```

Per default the same volume capacity and storage class are used for all FCCs.\
The capacity and storage class can also be set per FCC like so:

```yaml
fcc:
  instances:
    - vtssId: <FccId>
      vtssSecret: <FccSecret>
      storageClass: <overrideStorageClass>
      storageCapacity: <overrideCapacity>
```

> More configuration options are listed in the [parameter reference](ParameterReference.md).

## SCU Configuration

SCUs need to be setup to use byodc hosted FCC instances.

To do so the `FccUri` parameter needs to be set to `http://<FccId>` in the fiskaltrust.Portal.

![](images/HowToFCC-SCU-Configuration.png)

## Notes & Caveats

### Volumes

* Kubernetes will not automatically delete the persistent volumes if the FCC is removed from the configuration.\
  This means the `.fccdata` folder and thus the FCC can be saved if something is deleted or configured wrongly by mistake.

* Each FCC has its own persistent volume. The volume needs to be resized in the configuration if its capacity is reached.

* A storage class which allows resizing should be used.

* If a non POSIX compliant storage class is used (e.g. most network drives) the FCC will not be able to start (The FCC uses SQLite which causes problems when used on network drives).

---

The FCCs all have their own volume instead of a shared volume for all.\
This is because having one shared volume would force all FCC instances to run on the same node. (Disk volumes can mostly only be used by a single node (use accessMode `ReadWriteOnce`))

There is a [github discussion](https://github.com/fiskaltrust/product-de-bring-your-own-datacenter/discussions/66) regarding this choice. Feel free to chime in and offer your thoughts/wishes there.
