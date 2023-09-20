

# k8s dev notes

dev log notes on gan deployment

## mounting logs, data etc

just make a seperate pvc for each is the recommended way.

__NOTE__ we are _currently_ using a local store on micrk8s but really this can be replaced with some form of block storage via the config ideally i suppose this would be some kind of fibre channel mounted block store.


# subPaths and subPathExpr

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

have to use the `subPath` syntax.

This will create the paths that the `ENV_VAR` expands to onto the underlying file storage and mount this to the `mountPath`

so then given a `PV` that uses a path like `/usr/local/mount/k8s/src` on a `local` store and a `volumeMount` on a `PVC` from that `PV` using a `subPathExpr` like this: 
```yaml
      volumeMounts:
        - name: obs-src
          mountPath: /app
          subPathExpr: $(SRC_STORE)
```
with envs:
```yaml
        - name: SRC_STORE
          valueFrom:
            configMapKeyRef:
              name: paths-conf
              key: srcStore
```
and a cm:
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: paths-conf
data:
  dataStore: "ci/release/sql"
  logStore: "ci/release/obs"
  srcStore: "ci/release"
```

we end up with the path built up on the host of:
```
.
|__usr
	|__local
		|__mount
			|__k8s
				|__src
					|__ci
						|__release
```
and then whatever sits at the end of that path mounted into `/app` in the container

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

## dns

[dns debugging](https://kubernetes.io/docs/tasks/administer-cluster/dns-debugging-resolution/)

`kk exec -it test-observer --container sh-container -- cat /etc/resolv.conf`


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE5MTE3NjQwOTgsMTczMjY5MzA0MSwtMT
I2NzA4Njc2Nyw3Mzk0MzMyNzYsLTE3MDM5MDgyMjksLTEwMTU2
NjY2NTcsMTEzMDY2NjQwMCwtMTUyNTgzNDI5Niw2MTAyMTA2Nj
YsOTAwNjAwMjVdfQ==
-->