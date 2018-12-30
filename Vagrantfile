# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'json'

confDir = File.expand_path(File.dirname(__FILE__))
settingsJsonPath = confDir + "/settings.json"

Vagrant.configure(2) do |config|
  settings = JSON::parse(File.read(settingsJsonPath))
  config.vm.box = "ubuntu/bionic64"
  config.vm.network "private_network", ip: settings['ip']
  config.vm.provider "virtualbox" do |p|
    p.name = settings['name']
    p.memory = settings['memory']
    p.cpus = settings['cpus']
  end
  settings['synced-folders'].each do |folder|
    config.vm.synced_folder folder['from'], folder['to'], type: 'nfs', nfs_version: 3, nfs_udp: false
  end
  config.vm.provision "shell", inline: <<-SHELL
    wget -qO- https://get.docker.com/ | sh
    groupadd docker
    usermod -aG docker vagrant
    curl -L "https://github.com/docker/compose/releases/download/1.23.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    chmod +x /usr/local/bin/docker-compose
  SHELL
end
