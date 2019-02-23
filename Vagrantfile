# -*- mode: ruby -*-
# vi: set ft=ruby :

$SEC1_IP = "172.16.137.202"

ANSIBLE_RAW_SSH_ARGS = []

Vagrant.configure("2") do |config|
  config.vm.define "sec1" do |sec1|
    sec1.vm.box = "generic/ubuntu1804"
    sec1.vm.hostname = "sec1"
    sec1.vm.network "private_network", ip: $SEC1_IP

    sec1.vm.provider :virtualbox do |v, override|
      ANSIBLE_RAW_SSH_ARGS << " -o IdentityFile=./.vagrant/machines/sec1/virtualbox/private_key "
      v.gui = false
      v.customize ["modifyvm", :id, "--cpus", 1]
      v.customize ["modifyvm", :id, "--memory", 1024]
      v.customize ["modifyvm", :id, "--cableconnected1", "on"]
      v.customize ["modifyvm", :id, "--cableconnected2", "on"]
    end

    sec1.vm.provider :libvirt do |v, override|
      ANSIBLE_RAW_SSH_ARGS << " -o IdentityFile=./.vagrant/machines/sec1/libvirt/private_key "
      v.cpu_mode = 'custom'
      v.cpu_model = 'kvm64'
      v.cpus = 1
      v.memory = 1024
    end

    sec1.vm.provision "shell", inline: "apt-get install -y python"
    sec1.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/site.yml"
      ansible.config_file = "ansible/ansible.cfg"
      ansible.inventory_path = "ansible/inventory"
      ansible.become = true
      ansible.limit = "all"
      ansible.raw_arguments = ["--diff"]
      ansible.raw_ssh_args = ANSIBLE_RAW_SSH_ARGS
    end
  end
end
