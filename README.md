# docker-swarm-vagrant

This project aims to be a simple and fast solution to create and configure a whole [Docker Swarm cluster](https://docs.docker.com/engine/swarm/). without user interaction as several virtual machines.


### How it works

[VirtualBox](https://www.virtualbox.org/), [Vagrant](https://www.vagrantup.com/) and [Ansible](https://www.ansible.com/) have to be installed before you can create the Docker swarm cluster

After installing the previous mentioned dependencies, the cluster can be started as follows.
```bash
# Start the creation and configuration of the cluster
vagrant up --provision
# Check the current state of the VMs
vagrant status
```

On each VM is a Docker Engine running in "Swarm Mode".
There are Swarm Managers and Swarm Workers.
The Managers have IPs like `10.0.10.{i}` and hostnames like `manager-{i}` (where `{i}`  is the number of the manager). The workers have IPs like `10.0.11.{i}` and hostnames like `worker-{i}`. All nodes (managers and workers) are in the same IPv4 subnet `10.0.0.1/16`.

To SSH into a node, use `vagrant ssh`. E.g. `vagrant ssh manager-1`

There is also [Avahi/mDNS](http://avahi.org/) installed on every VM. Therefore hostnames like `manager-{i}.local` will be resolved to the correct IPs of the VM and can be used by the host system, if avahi is also installed on the host.

The swarm manager `manager-1` is the leading manager of the swarm cluster.
On this node is also [Portainer](https://portainer.io/), a management UI for Docker, installed. It can be reached at `http://manager-1.local:9000/` through a webbroser.
Portainer is also capable of managing the Docker swarm as a whole, instead of only the Docker engine on manager-1.

## Feedback
Bug reports, feature requests, pull requests and feedback in general is always welcome!

## License
[MIT](LICENSE)  
Copyright &copy; 2017 Simon Schürg
