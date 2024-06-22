## PV / PVC

- Storage class - stores connection params
- PersistentVolumeClaim - describe requirements for volume
- PersistentVolume - stores volume params and status

```yaml
volumes:
  - name: config
    configMap:
      name: fileshare
  - name: data # as link
    persistentVolumeClaim: # volume type
      claimName: fileshare # claim name
```

- space
- file system
- readWriteOnce / readWriteMany / readManyWriteOnce

Deleting pvc
Depends on StorageClass:

- ReclaimPolicy
  - Delete(pv'll be deleted)
  - Retain(pv'll not be touched) - to attach, change status from Released to Available. Create PVC for that PV

Edit volume:

```shell
k edit pvc fileshare
```
