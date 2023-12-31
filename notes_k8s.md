

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

## registries

see [docs on private registries](https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/)

need a secret for the login auth stuff for a private registry, i.e `build.uat.gan`

* login: `docker login ...`
  * generates a `~/.docker/config.json`
* generate a secret from this file
  * `kubectl create secret generic regcred --from-file=.dockerconfigjson=~/.docker/config.json --type=kubernetes.io/dockerconfigjson --dry-run=server -o yaml`
  NB: `--dry-run=server -o yaml` just outputs the yml to `stdout` and ensures its not actually created
 * use the `regcred` in a pod: 
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: private-reg
spec:
  containers:
  - name: private-reg-container
    image: <your-private-image>
  imagePullSecrets:
  - name: regcred
```
 
the actual  creds file should look similar to: 
```yaml
apiVersion: v1
data:
  .dockerconfigjson: ewoJImF1dGhzIjogewoJCSJidWlsZC51YXQuZ2FuIjoge30sCgkJImh0dHBzOi8vaW5kZXguZG9ja2VyLmlvL3YxLyI6IHt9Cgl9LAoJImNyZWRzU3RvcmUiOiAiZGVza3RvcCIsCgkiZXhwZXJpbWVudGFsIjogImRpc2FibGVkIiwKCSJjdXJyZW50Q29udGV4dCI6ICJkZXNrdG9wLWxpbnV4IiwKCSJwbHVnaW5zIjogewoJCSIteC1jbGktaGludHMiOiB7CgkJCSJlbmFibGVkIjogInRydWUiCgkJfQoJfQp9
kind: Secret
metadata:
  creationTimestamp: "2023-09-21T10:43:24Z"
  name: regcred
  namespace: default
  selfLink: /api/v1/namespaces/default/secrets/regcred
  uid: 63e37d67-71f4-4360-9ea1-2cc4866b84be
type: kubernetes.io/dockerconfigjson 
```

also `microk8s` needs to have this insecure registry setup in its own config
see [configuring private registries](https://microk8s.io/docs/registry-private)
<!--stackedit_data:
eyJoaXN0b3J5IjpbNjU5MTcyOTI1LC02NjY3MTExMjcsLTMzMD
Y5MTY3MSwtMTIxNTE4Mzc0MywtMTkxMTc2NDA5OCwxNzMyNjkz
MDQxLC0xMjY3MDg2NzY3LDczOTQzMzI3NiwtMTcwMzkwODIyOS
wtMTAxNTY2NjY1NywxMTMwNjY2NDAwLC0xNTI1ODM0Mjk2LDYx
MDIxMDY2Niw5MDA2MDAyNV19
-->