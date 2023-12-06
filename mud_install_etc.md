
## MUD 

### pnpm and node
* setup `nodenv` for node versioning `brew instal nodev`
* upgrade the install
		`$ brew upgrade nodenv node-build`
* install a local version (to root of project)
  `nodenv install <version>`
  `nodenv local <version>`
  NB `<version>` must be `18.x`

* install `pnpm`   
	* `curl -fsSL https://get.pnpm.io/install.sh | sh -`

## MUD

this is pretty broken as it goes, the quickstart docs aren't right. (there is an updated version available [here](https://mud-docs-4u4f02n5n-latticexyz.vercel.app/templates/typescript/getting-started) which is also wrong for me at least.

seems to be related to`mprocs` and how its is called by the `pnpm` process. For some reason it seems that it doesnt actually call it or perhaps it just doesn't pick up the config files, either way sucky.... 

workaround is:
 
needs also:
* install `forge` might not even need this as it should be `pnpm`'d
* install `mprocs`
	* `brew install mprocs`
* install `libusb` (albeit this may be installed already... seems related to GrPC no idea really)

1. create a project:
	`pnpm create mud@next <myProject>`
2. `cd` into `myProject`
3. `mprocs --config ./mprocs.yaml`
4. goto `localhost:3000`




## `pnpm build mud:next` errors

errors on build with

 ```stderr: 'dyld[6735]: Library not loaded: /usr/local/opt/libusb/lib/libusb-1.0.0.dylib\n' +
    '  Referenced from: <DBA4C99C-39D9-34D3-8B50-18F24DB02C69> /Users/tims/.foundry/bin/forge\n' +
    "  Reason: tried: '/usr/local/opt/libusb/lib/libusb-1.0.0.dylib' (no such file), '/System/Volumes/Preboot/Cryptexes/OS/usr/local/opt/libusb/lib/libusb-1.0.0.dylib' (no s
uch file), '/usr/local/opt/libusb/lib/libusb-1.0.0.dylib' (no such file), '/usr/local/lib/libusb-1.0.0.dylib' (no such file), '/usr/lib/libusb-1.0.0.dylib' (no such file, no
t in dyld cache)"
```

really fucking annoying

* __fix__  with `brew install libusb`
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTUzMDg0NjY4MSwxMTIwNTIxMTEyLC0xOT
c0MTM2Mzg4LDIwMzM2NzM0NzgsLTExODI5Njc4MDgsLTQ5ODI4
MDIxMywtMjAyMzMwOTczNF19
-->