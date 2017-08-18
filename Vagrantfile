# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrant Docs: https://docs.vagrantup.com
Vagrant.configure("2") do |config|

  # Discover Vagrant Boxes: https://atlas.hashicorp.com/search
  config.vm.box = "ubuntu/xenial64"
  config.vm.box_check_update = true

  config.vm.graceful_halt_timeout = 30
  config.vm.hostname = "vagrant-vm"

  # Network
  # config.vm.network "forwarded_port", guest: 80, host: 8080
  # config.vm.network "private_network", ip: "192.168.33.10"
  # config.vm.network "public_network"

  # Volumes
  # config.vm.synced_folder "../data", "/vagrant_data"

  # VirtualBox
  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    # vb.memory = "1024"
  end

  # Provisioning
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
  SHELL

end
