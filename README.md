Bootstrap a cluster with Vagrant
================================

This project provides the Vagrantfile and Ansible playbooks to bootstrap a
cluster environment from scratch.

The Vagrantfile will create a new virtual machine with a network interface
connected to the public network, so the VM will act as a node directly
connected to the network of the cluster.

The playbooks will install and configure Cobbler and Dnsmasq in the virtual
machine, to allow easy deployment of the physical nodes of the cluster.

Requirements
------------

A first head node must be installed with any Linux OS that supports Vagrant and
the libvirt/QEMU/KVM hypervisor. It is possible to pick any nodes which may be
reinstalled later once the cluster environment is ready.

The head node must act as a network gateway to provide access to the external
resources required to deploy the environment (e.g. distro packages).

Instructions
------------

1. Clone this project to your head node.
2. Configure the Ansible inventory to match your needs.
3. Enter the directory and run `vagrant up`.
4. Enable Netboot in Cobbler and start the deployment of your servers.
