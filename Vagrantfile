# -*- mode: ruby -*-
# vi: set ft=ruby :

$SEC1_IP = "172.16.137.202"

ANSIBLE_RAW_SSH_ARGS = []

ANSIBLE_RAW_SSH_ARGS << " -o IdentityFile=./.vagrant/machines/sec1/virtualbox/private_key "

Vagrant.configure("2") do |config|
  config.vm.define "sec1" do |sec1|
    sec1.vm.box = "ubuntu/bionic64"
    sec1.vm.hostname = "sec1"
    sec1.vm.network "private_network", ip: $SEC1_IP

    sec1.vm.provider :virtualbox do |v, override|
      v.gui = false
      v.customize ["modifyvm", :id, "--cpus", 1]
      v.customize ["modifyvm", :id, "--memory", 1024]
      v.customize ["modifyvm", :id, "--cableconnected1", "on"]
      v.customize ["modifyvm", :id, "--cableconnected2", "on"]
    end

    sec1.vm.provision "shell", inline: "apt-get install -y python"
    sec1.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/site.yml"
      ansible.config_file = "ansible/ansible.cfg"
      ansible.inventory_path = "ansible/inventory"
      ansible.become = true
      ansible.limit = "all"
      ansible.raw_ssh_args = ANSIBLE_RAW_SSH_ARGS
    end
  end
end
