# -*- mode: ruby -*-
# vi: set ft=ruby :

DOMAIN_NAME = 'xybits.vagrant'
SERVER = {
  # Base Specification
  :box_name => 'centos/7',
  :memory   => 1024,
  :cpus     => 1,
  # Ansible Specification
  :roles_path => 'ansible-roles/',
  :inventory  => 'ansible-data/inventories/cluster/hosts.yml',
  :playbook   => 'ansible-data/playbooks/cluster/mongo_servers.yml',
}

INSTANCES  = {
  :mongo01 => {
    :private_ip => '192.168.53.101',
  },
  :mongo02 => {
    :private_ip => '192.168.53.102',
  },
  :mongo03 => {
    :private_ip => '192.168.53.103',
  },
  :mongo04 => {
    :private_ip => '192.168.53.104',
    :memory   => 512,
  },
}

servers = Hash.new
INSTANCES.each do |server, spec|
  servers["#{server}"] = SERVER.merge(spec)
end

Vagrant.configure("2") do |config|
  config.vagrant.plugins     = [ "vagrant-vbguest" ]
  config.vbguest.auto_update = false

  servers.each do |name, machine|
    config.vm.define name, autostart: false do |node|
      full_name = "#{name}".gsub(/_/, "-") + "." + DOMAIN_NAME

      node.vm.box      = machine[:box_name]
      node.vm.hostname = full_name

      # Set Private IP
      if machine[:private_ip] != nil
        node.vm.network "private_network", ip: machine[:private_ip]
      end

      # Provider
      node.vm.provider :virtualbox do |vb|
        vb.name   = full_name
        vb.memory = machine[:memory]
        vb.cpus   = machine[:cpus]
      end

      # Provision
      node.vm.provision "ansible" do |ans|
        ans.verbose  = "vv"
        ans.limit    = machine[:private_ip]
        ans.playbook = machine[:playbook]
        ans.inventory_path    = machine[:inventory]
        ans.galaxy_roles_path = machine[:roles_path]
      end
    end
  end
end

