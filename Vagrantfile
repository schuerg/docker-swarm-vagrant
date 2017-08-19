# -*- mode: ruby -*-
# vi: set ft=ruby :


VAGRANTFILE_API_VERSION = "2"

manager = (1..2).map { |name| "manager-" + name.to_s }
worker = (1..3).map { |name| "worker-" + name.to_s }

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
  manager.each_with_index do |name, i|
    config.vm.define name do |node|
      node.vm.hostname = name
      node.vm.network :private_network,
        ip: "10.0.10.#{i + 1}",
        netmask: "255.255.0.0"
      # node.vm.network "forwarded_port", guest: 80, host: 8080
    end
  end

  # Docker Swarm Worker VMs
  worker.each_with_index do |name, i|
    config.vm.define name do |node|
      node.vm.hostname = name
      node.vm.network :private_network,
        ip: "10.0.11.#{i + 1}",
        netmask: "255.255.0.0"
      # node.vm.network "forwarded_port", guest: 80, host: 8080
    end
  end

  # Ansible provisioning from the Vagrant Host
  config.vm.provision "ansible" do |ansible|
    # ansible.limit = "vagrant"
    ansible.playbook = "ansible/playbook.yml"
    # ansible.inventory_path = "ansible/inventory"
    ansible.galaxy_roles_path = "ansible/roles"
    ansible.galaxy_role_file = "ansible/requirements.yml"
    # ansible.vault_password_file = "ansible/.vault_pass"
    ansible.verbose = "v"

    ansible.groups = {
      "swarm-leader" => [ manager[0] ],
      "swarm-manager" => manager,
      "swarm-worker" => worker
    }

  end

end
