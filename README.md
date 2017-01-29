# Ansible Super LAMP Environment Provisioner

Ansible provisioner for LAMP environments using Apache2, Php-fpm 7.1, MySQL 5.7, Xdebug, Composer, Git, Redis and Blackfire.

## Dependencies

- VirtualBox. Install with: 

        *sudo apt-get update*
        *sudo apt-get install virtualbox*

- Vagrant. Download latest version from official website and install.

- Ansible. Install with: 
         
         *sudo apt-get update*         
         *sudo apt-get install ansible*

## Installation

1. Copy *vagrant_inventory.example.yml* to *vagrant_inventory.yml* and modify what required.

2. Be sure to replace on *ansible/hosts*:

- *superprovisioner* to the same value as *name* on vagrant_inventory.yml
- *ansible_ssh_port* to the same value as *port_host* on vagrant_inventory.yml
- *machine name* on *ansible_ssh_private_key_file* to the same value as *name* on vagrant_inventory.yml
