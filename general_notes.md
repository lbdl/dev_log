## registries (private)
see [docs](https://docs.docker.com/registry/insecure/)

[pulling private images](https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/)

using `build.uat.gan` at present as a test env for images

## CMD
when we use an `ENTRYPOINT` then the `CMD` essentially acts as an array of flags/params to the command listed as `ENTRYPOINT`

## gan registry
sits at `build.uat.gan`
ios stuff under `/build/ios`

## pushing images

tag as `build.uat.gan/build/ios/myImage:0.1.0`
`docker login`
`docker push build.uat.gan/build/ios/myImage:0.1.0`

we _ALWAYS_ push the new tag as itself and `latest`
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTA3NjAyNTc3N119
-->