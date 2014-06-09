#Vagrant + Ansible + Docker + btrfs

The Vagrantfile in this repository will create a virtual machine, add a virtual box disk to it (/dev/sdb in the OS), install btrfs-tools, format and mount /dev/sdb on /var/lib/docker as btrfs, then install docker and configure it to start with the btrfs driver.

