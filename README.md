#Vagrant + Ansible + Docker + btrfs

The Vagrantfile in this repository will create an Ubuntu 14.04/Trusty virtual machine, add a virtual box disk to it (/dev/sdb in the OS), install btrfs-tools, format and mount /dev/sdb on /var/lib/docker as btrfs, then install docker and configure it to start with the btrfs driver. 

Note that it will create a .vdi file for /dev/sdb.

```bash
$ vagrant up
SNIP!
$ vagrant ssh
SNIP!
vagrant@host1:~$ sudo btrfs filesystem df /var/lib/docker
Data, single: total=1.01GiB, used=337.43MiB
System, DUP: total=8.00MiB, used=16.00KiB
System, single: total=4.00MiB, used=0.00
Metadata, DUP: total=1.00GiB, used=1.88MiB
Metadata, single: total=8.00MiB, used=0.00
vagrant@host1:~$ ps aux | grep [d]ocker
root      4984  0.1  0.6 304032  9844 ?        Sl   23:44   0:00 /usr/bin/docker.io -d -s btrfs
```

Note that this playbook also sets up an alias for docker.

```bash
vagrant@host1:~$ alias | grep docker
alias docker='sudo docker.io'
```

## Requirements

- Vagrant 1.5+
- Ansible installed on the Vagrant Host

