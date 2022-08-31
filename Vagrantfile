# -*- mode: ruby -*-
# vi: set ft=ruby :
require 'yaml'

settings = YAML.load_file('vagrant.yml')
vm_public_dev = settings['bootstrap_public_dev']
vm_public_ip = settings['bootstrap_public_ip']
vm_public_netmask = settings['bootstrap_public_netmask']
vm_ssh_key_type = settings['bootstrap_ssh_key_type']

Vagrant.configure("2") do |config|
  config.vm.box = "generic/debian11"

  config.vm.define "bootstrap" do |t|
    t.vm.hostname = "bootstrap"
  end

  config.vm.synced_folder "./", "/vagrant"

  if vm_ssh_key_type
    config.vm.provision "file",
      source: "~/.ssh/id_" + vm_ssh_key_type,
      destination: "$HOME/.ssh/id_" + vm_ssh_key_type
  end

  config.vm.provision "ansible_local" do |ansible|
    ansible.limit = "localhost,all"
    ansible.playbook = "ansible/install_role.yml"
    ansible.verbose = true
  end

  config.vm.provision "ansible_local" do |ansible|
    ansible.limit = "all"
    ansible.inventory_path = "ansible/inventory.yml"
    if Dir.exist?('ansible/inventory/')
      ansible.inventory_path = "ansible/inventory/"
    end
    ansible.playbook = "ansible/provision.yml"
    ansible.verbose = true
  end

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  config.vm.network "public_network",
    dev: vm_public_dev,
    ip: vm_public_ip,
    netmask: vm_public_netmask

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
end
