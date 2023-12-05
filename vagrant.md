# vagrant

changing versions
```
vagrant plugin uninstall vagrant-vmware-desktop
vagrant plugin install --plugin-version=3.0.1 vagrant-vmware-desktop
```

## issues with ssh and connectivity

seems the vmnet doesn't reset itself or something.

* the fix is to delete all the machines under `/.vagrant/machines` and further remove the vagrant generated inventory `.vagrant/provisioners/ansible/inventory/...`

this seems to effectively reset the networking stack

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
`ansible-galaxy role install elliotweiser.osx-commandline-tools`

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0NDMzMjU3MTddfQ==
-->