

# k8s dev notes

dev log notes on gan deployment

##


# subPaths

see https://kubernetes.io/docs/concepts/storage/volumes/#using-subpath

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
 
## initContainers

handy for git clones etc
especially using `git-sync` as a sidecar app
## git-sync

side car, doesn't like top level folders in the repo, i.e. a structure like
```
.
|
|_src
	|_ file1
	|_ file2
	|_ ...
```
just fails silently or rather if you do `ls` then this doesn't show the file's just the hash. anyway. dont
> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTcxMzUyOTc4OSw5MDA2MDAyNV19
-->