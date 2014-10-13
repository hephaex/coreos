# CoreOS Vagrant

 This repo provides a Vagrantfile to create a CoreOS virtual machine using the VirtualBox hypervisor.
When CoreOS setting complete, you'll have a single CoreOS virtual machine running on your local machine.

## Streamlined setup

* Install Hypervisor & Vagrant
 - [VirtualBox][virtualbox] 4.3.14 or greater.
 - [Vagrant][vagrant] 1.6.5 or greater.

## Clone my CoreOS Vagrant repo

```
$ git clone https://github.com/hephaex/coreos.git
$ cd coreos
```

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

* start VM
```
vagrant up
```

* Doing "vagrant up" you'll face on password request for shared folder access"

* connect CoreOS VM
```
vagrant ssh
```

## Docker Forwarding

To use the docker on your VM, you'll export Docker_HOST setting.

```
core@core-01 ~ $ export DOCKER_HOST=tcp://localhost:2375
```

## CentOS Docker
* Download CentOS image for Docker
```
docker pull centos:centos6
```

* You can choise varios centos version for docekr.
 - CentOS5.x: docker pull centos:centos5
 - CentOS6.x: docker pull centos:centos6
 - CentOS7.x: docker pull centos:latest

* check docker images
```
core@core-01 ~ $ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
centos              centos6             68edf809afe7        11 days ago         212.7 MB
```

* delete docker image
```
core@core-01 ~ $ docker rmi centos:latest
Untagged: centos:latest
Deleted: 87e5b6b3ccc119ebfe9344583fb3f77804d6e3d9a3553d916fbf807028310e8e
```

* docker process check & kill all
```
core@core-01 ~ $ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                       PORTS               NAMES
abc10c9dce94        centos:centos6      "/bin/bash"         About an hour ago   Exited (127) 5 seconds ago                       mad_elion
core@core-01 ~ $ docker rm `docker ps -a -q`
abc10c9dce94
```

* run CentOS6
```
core@core-01 ~ $ docker run -i -t centos:centos6 /bin/bash
bash-4.1# uname -a
Linux abc10c9dce94 3.16.2+ #2 SMP Tue Oct 7 01:50:34 UTC 2014 x86_64 x86_64 x86_64 GNU/Linux
```

* update RPMs
```
bash-4.1# yum update
```

* install RPMs (ex. ruby-1.8.7)
```
bash-4.1# yum install ruby
Running Transaction
 Installing : compat-readline5-5.2-17.1.el6.x86_64                         1/3
 Installing : ruby-libs-1.8.7.352-13.el6.x86_64                            2/3
 Installing : ruby-1.8.7.352-13.el6.x86_64                                 3/3
 Verifying  : ruby-1.8.7.352-13.el6.x86_64                                 1/3
 Verifying  : compat-readline5-5.2-17.1.el6.x86_64                         2/3
 Verifying  : ruby-libs-1.8.7.352-13.el6.x86_64                            3/3

Installed:
  ruby.x86_64 0:1.8.7.352-13.el6

Dependency Installed:
  compat-readline5.x86_64 0:5.2-17.1.el6   ruby-libs.x86_64 0:1.8.7.352-13.el6

Complete!
bash-4.1# ruby --version
ruby 1.8.7 (2011-06-30 patchlevel 352) [x86_64-linux]
```
