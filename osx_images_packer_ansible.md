# building osx images 
## commands
`softwareupdate --list-full-installers`
gets list of installers

¿ _why is it so hard_ ?  ¡ because its __apple__ stupid !

`softwareupdate --fetch-full-installer --full-installer-version 11.6.1` will then fetch the installer for 11.6.1

Monterey is `12.7` 
Ventura is `13.6`

## mist-cli
easier option for downloading installers
`brew install mist-cli`

## boot_command
plugin `vmware-iso` the vmware builder version 1.0.10 is broken it seems on OSx 13 anyway and causes the `curl` command in the `boot_command` to endlessly fail as it thinks its receiving http0.9, its not.
The fix is to pin the plugin to `1.0.6` other version sub `1.0.10` may also work but I haven't tested this out.

## network config - vagrant

seems busted - where is the config kept for the bridging etc NAT isn't right either that or the port is already mapped or something...

## ansible provisioning

brew role not working...
trying [wayofdev homebrew role](https://github.com/wayofdev/ansible-role-homebrew#-requirements)
needs `jmespath`
 ```python -m pip install jmespath```
also uses
`wayofdev.homebrew`
`ansible-galaxy role search wayofdev`

#### homebrew role deps
`ansible-galaxy role install wayofdev.homebrew`
`ansible-galaxy role install elliotweiser.osx-command-line-tools`

## galaxy roles

`ansible-galaxy collection list`

### searching
`ansible-galaxy role search foo --author bar`

## network-config fusion 13


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTM4MTQ5Mzg1OF19
-->