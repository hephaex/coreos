# CoreOS Vagrant

 This repo provides a Vagrantfile to create a CoreOS virtual machine using the VirtualBox hypervisor.
When CoreOS setting complete, you'll have a single CoreOS virtual machine running on your local machine.

## Streamlined setup

* Install Hypervisor & Vagrant
 - [VirtualBox][virtualbox] 4.3.14 or greater.
 - [Vagrant][vagrant] 1.6.5 or greater.

## Edit your own virtual machine enviroment.

* open & edit Vagrantfile

```
# vi Vagrantfile
```

### VM setting
* Number of VM instances
```
$num_instances = 1
```

* CoreOS Channel setting
 - there are three kinds of distribute channel. "Stable","Alpha",and "Beta".
```
* $update_channel = "stable"
```
* VM memory setting
```
$vb_memory = 2048
```

* VM cpus
```
$vb_cpus = 2
```

* Shared Folder setup

```
config.vm.network "private_network", ip: "172.17.8.150"
config.vm.synced_folder ".", "/home/{your folder}/share", id: "core", :nfs => true,  :mount_options   => ['nolock,vers=3,udp']
```

## Initialization virtual machine on local machine.

```
vagrant up
vagrant ssh
```


