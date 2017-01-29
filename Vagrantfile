# -*- mode: ruby -*-
# # vi: set ft=ruby :

# Specify minimum Vagrant version and Vagrant API version
Vagrant.require_version ">= 1.6.0"
VAGRANTFILE_API_VERSION = "2"

# Require YAML module
require 'yaml'

# Read YAML file with box details
inventory = YAML.load_file(File.join(File.dirname(__FILE__), 'vagrant_inventory.yml'))

# Create boxes
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

    # SSH settings
    config.ssh.insert_key = true
    config.ssh.forward_agent = true

    # Optimize network connection
    config.vm.provider "virtualbox" do |v|
       v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
       v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    end

    # Iterate through entries in YAML file
    inventory.each do |inventory|
        config.vm.define inventory["name"] do |machine|

            # Box and bootstrap settings
            machine.vm.hostname = inventory["name"]
            machine.vm.box = inventory["box"]
            machine.vm.boot_timeout = 600
            machine.vm.box_check_update = false

            # Network settings
            machine.vm.network "private_network", ip: inventory["ip"]

            # Forwarded port settings
            machine.vm.network "forwarded_port", guest: inventory["port_guest"], host: inventory["port_host"], id: 'ssh'

            # Sync folder settings
            machine.vm.synced_folder ".", inventory["target_folder"], nfs: true

            # Virtal machine provider settings
            machine.vm.provider :virtualbox do |vb|
                vb.name = inventory["name"]

                # Display the VirtualBox GUI when booting the machine
                vb.gui = false

                # Customize the amount of memory on the VM:
                vb.memory = inventory["ram"]
            end

            # Provisioning settings
            machine.vm.provision "ansible" do |ansible|
                ansible.inventory_path = './ansible/hosts'
                ansible.playbook       = inventory["ansible_playbook"]
                ansible.limit          = inventory["ansible_machine_name"]
                ansible.verbose        = 'false'
                ansible.extra_vars     = inventory["ansible_extra_vars"]
                ansible.verbose        = inventory["ansible_verbose"]
            end
        end
    end
end

