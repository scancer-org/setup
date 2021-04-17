# -*- mode: ruby -*-

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = "ubuntu/focal64"
  config.vm.network "private_network", ip: "192.168.33.16"

  ENV['LC_ALL']="en_US.UTF-8"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.cpus = 2
  end

  config.vm.provision :ansible do |ansible|
    ansible.playbook = "site.yml"
    ansible.inventory_path = "servers/local"
    ansible.limit = "all"
  end

end
