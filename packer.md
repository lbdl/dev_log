# packer
make sure that the vmware plugin exists
`packer plugins install github.com/hashicorp/vmware`

also now see the new packer config section at the head of the file, this means it is possible to run `packer init myTemplate.hcl` this is what we now use

## validate

validate with:
`packer validate myTemplate.hcl`
syntax only
`packer validate --syntax-only myTemplate.hcl`

## FIXED ~~wierd http 0.9 errors in boot command stuff~~

I assume this is because we now need the following in the `vmx_data` block
`    "ethernet0.virtualDev"                     = "vmxnet3"`

previous to this the `boot_command` fails when trying to fetch the ssh packages, and it fails on `curl` complaining about 0.9 not allowed, as far as I can find this means that curl just doesn't like it and I cant see any reason for this other than _perhaps_ that missing param

### fix :warning:
:warning: downgrade the vmware-iso plugin to `< 1.10` 

## getting board id

`ioreg -l | grep -i board-id`

## device id
`/usr/libexec/remotectl get-property localbridge HWModel`

## nc testing
tested with nc

`nc -z -v 172.16.19.1 8977`
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTE3MjI2NTY1Nl19
-->