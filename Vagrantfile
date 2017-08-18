# -*- mode: ruby -*-
# vi: set ft=ruby :


VAGRANTFILE_API_VERSION = "2"

# Vagrant Docs: https://docs.vagrantup.com
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Discover Vagrant Boxes: https://atlas.hashicorp.com/search
  config.vm.box = "ubuntu/xenial64"
  config.vm.box_check_update = true

  # VirtualBox
  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    # vb.memory = "1024"
  end

  config.vm.synced_folder ".", "/vagrant", type: "rsync",
    rsync__exclude: ".git/",
    rsync__args: ["--verbose", "--archive", "--delete", "-z"]

  # Docker Swarm Manager VMs
  (1..2).each do |i|
    config.vm.define "manager-#{i}" do |node|
      node.vm.hostname = "manager-#{i}"
      node.vm.network :private_network,
        ip: "10.0.10.#{i}",
        netmask: "255.255.0.0"
      # node.vm.network "forwarded_port", guest: 80, host: 8080
    end
  end

  # Docker Swarm Worker VMs
  (1..3).each do |i|
    config.vm.define "worker-#{i}" do |node|
      node.vm.hostname = "worker-#{i}"
      node.vm.network :private_network,
        ip: "10.0.11.#{i}",
        netmask: "255.255.0.0"
      # node.vm.network "forwarded_port", guest: 80, host: 8080
    end
  end

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update && apt-get install -y avahi-daemon libnss-mdns
  SHELL

  # Ansible provisioning from the Vagrant Host
  config.vm.provision "ansible" do |ansible|
    # ansible.limit = "vagrant"
    ansible.playbook = "ansible/playbook.yml"
    # ansible.inventory_path = "ansible/inventory"
    ansible.galaxy_roles_path = "ansible/roles"
    ansible.galaxy_role_file = "ansible/requirements.yml"
    # ansible.vault_password_file = "ansible/.vault_pass"
    ansible.verbose = "v"
  end

end
