
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

:warning:
`mprocs` seems to install a ARM64 binary when run from `pnpm` on an `x86` machine.

workaround is:
 
needs alsoinstall deps:
* install `forge` might not even need this as it should be `pnpm`'d
* install `mprocs`
	* `brew install mprocs`
* install `libusb` (albeit this may be installed already... seems related to GrPC no idea really)

1. create a project:
	`pnpm create mud@next <myProject>`
2. `cd` into `myProject`
3. `mprocs --config ./mprocs.yaml`
4. goto `localhost:3000`

5. receive :bacon:


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

## custom types in MUD table types
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQ4NDIzODEzLDE1MzA4NDY2ODEsMTEyMD
UyMTExMiwtMTk3NDEzNjM4OCwyMDMzNjczNDc4LC0xMTgyOTY3
ODA4LC00OTgyODAyMTMsLTIwMjMzMDk3MzRdfQ==
-->