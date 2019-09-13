# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  # ensure to have vagrant-vbguest installed
  required_plugins = %w( vagrant-vbguest )
  _retry = false
  required_plugins.each do |plugin|
    unless Vagrant.has_plugin? plugin
      system "vagrant plugin install #{plugin}"
      _retry = true
    end
  end
  if (_retry)
    exec "vagrant " + ARGV.join(' ')
  end

  # graylog VM
  config.vm.define "graylog" do |graylog|
    graylog.vm.box = "debian/stretch64"
    graylog.vm.hostname = "graylog"
    # VM options
    graylog.vm.provider "virtualbox" do |vb|
      vb.memory = "4096"
    end
    # forward some ports
    graylog.vm.network "forwarded_port", guest: 9000, host: 9000 # graylog
    graylog.vm.network "forwarded_port", guest: 3000, host: 3000 # grafana
    # private network
    graylog.vm.network "private_network", ip: "192.168.1.10"
    # update system
    graylog.vm.provision "shell", inline: <<-SHELL
      echo "set grub-pc/install_devices /dev/sda" | debconf-communicate
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
    client.vm.network "forwarded_port", guest: 80, host: 8080 # http
    # private network
    client.vm.network "private_network", ip: "192.168.1.11"
    # update system
    client.vm.provision "shell", inline: <<-SHELL
      echo "set grub-pc/install_devices /dev/sda" | debconf-communicate
      apt-get update && apt-get upgrade -y
    SHELL
    # prepare client
    client.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "client.yml"
    end
  end

end
