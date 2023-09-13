

# k8s dev notes

dev log notes on gan deployment

## mounting logs, data etc

just make a seperate pvc for each is the recommended way

__NOTE__ we are _currently_ using a local store on micrk8s but really this can be replaced with some form of block storage via the config ideally i suppose this would be some kind of fibre channel mounted block store.


# subPaths

see [k8s docs](https://kubernetes.io/docs/concepts/storage/volumes/#using-subpath)

cant just do 

```yaml
      volumeMounts:
        - name: obs-src
          mountPath: /usr/ci
        - name: obs-src
          mountPath: "$(LOG_STORE)"
        - name: obs-src
          mountPath: "$(DATA_STORE)"
```
this fails and only the top path gets mounted

have to use the `subPath` syntax

* docs recommend that don't use subPath in productions anyway

## PVC's and PV's

1 to 1 mapping between `PV` and `PVC` and again between `PV` and `StorageClass`
 
## initContainers

handy for git clones etc
especially using `git-sync` as a sidecar app
## git-sync

sidecar app. doesn't like top level folders in the repo, i.e. a structure like
```
.
|
|__src
	|__ file1
	|__ file2
	|__ ...
```
just fails silently or rather if you do `ls` then this doesn't show the file's just the hash. anyway. dont
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTEzMDY2NjQwMCwtMTUyNTgzNDI5Niw2MT
AyMTA2NjYsOTAwNjAwMjVdfQ==
-->