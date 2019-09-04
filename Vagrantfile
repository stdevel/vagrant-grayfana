# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  # graylog VM
  config.vm.define "graylog" do |graylog|
    graylog.vm.box = "debian/stretch64"
    graylog.vm.hostname = "graylog"
    # VM options
    graylog.vm.provider "virtualbox" do |vb|
      vb.memory = "4096"
    end
    # forward some ports
    graylog.vm.network "forwarded_port", guest: 9000, host: 9000   # http
    graylog.vm.network "forwarded_port", guest: 443, host: 8443  # https
    # private network
    graylog.vm.network "private_network", ip: "192.168.1.10"
    # update system
    graylog.vm.provision "shell", inline: <<-SHELL
      apt-get update && apt-get upgrade -y
    SHELL
    # install graylog
    graylog.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "graylog.yml"
    end
  end

  # client VM
  config.vm.define "client" do |client|
    client.vm.box = "debian/stretch64"
    client.vm.hostname = "client"
    # VM options
    client.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
    end
    # forward some ports
    # client.vm.network "forwarded_port", guest: 80, host: 8080   # http
    # client.vm.network "forwarded_port", guest: 443, host: 8443  # https
    # private network
    client.vm.network "private_network", ip: "192.168.1.11"
    # update system
    client.vm.provision "shell", inline: <<-SHELL
      apt-get update && apt-get upgrade -y
    SHELL
    # prepare client
    client.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "client.yml"
    end
  end

end
