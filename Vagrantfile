# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.define "dev" do |dev|
    dev.vm.box = "webhippie/opensuse-13.2"
  	dev.vm.synced_folder ".", "/data/docker/compose/quay-registry"
  	dev.vm.provider "virtualbox" do |v|
            v.memory = 2048
  	end
  	dev.vm.network :forwarded_port, host: 2222, guest: 22, id: "ssh", auto_correct: true
  	dev.vm.network "private_network", ip: "192.168.50.90"
  	dev.vm.hostname = "docker-ctlq.pictet.io"
    dev.vm.provision "shell", inline: <<-SHELL
    zypper in -y docker python-pip
    service docker start
    pip install docker-compose
    mkdir -p /data/postgres
    mkdir -p /data/quay/storage
    mkdir -p /data/quay/config
     SHELL
  end
  
 
end
