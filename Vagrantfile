# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

boxes = [
    { 
        :name => :host1, 
        :pubip => '192.168.33.89',
        :privip => '10.2.0.89',
        :box => 'ubuntu/trusty64',
        :vbox_config => [
            { '--memory' => '1536' }
        ],
    }, 
]

Vagrant.configure("2") do |config|

    Vagrant.require_version ">= 1.5.0"

    if Vagrant.has_plugin?("vagrant-cachier")
        config.cache.scope = :box
    end

    boxes.each do |opts|
        config.vm.define opts[:name] do |config|
            # Box basics
            config.vm.hostname = opts[:name]
            config.vm.box = opts[:box]
            config.vm.network :private_network, ip: opts[:pubip]
            config.vm.network :private_network, ip: opts[:privip]
            config.ssh.forward_agent = true

            # VirtualBox customizations
            unless opts[:vbox_config].nil?
                config.vm.provider :virtualbox do |vb|

                    # vboxmanage
                    opts[:vbox_config].each do |hash|
                        hash.each do |key, value|
                            vb.customize ['modifyvm', :id, key, value]
                        end
                    end
                end
            end   

        config.vm.provider "virtualbox" do |vb|
            lfs_disk = "#{opts[:name]}" + ".vdi"
            unless File.exist?(lfs_disk) 
                vb.customize ['createhd', '--filename', lfs_disk, '--size', 20 * 1024] 
            end 
             vb.customize ['storageattach', :id, '--storagectl', 'SATAController', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', lfs_disk] 
        end

        config.vm.provision "ansible" do |ansible|
            ansible.playbook = "playbook/site.yml"
        end
    end


    end
end
