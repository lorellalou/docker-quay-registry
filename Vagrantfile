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
  
  config.vm.define "exoscale" do |config|
  	config.vm.provider :cloudstack do |cloudstack, override|
      cloudstack.api_key    = ENV['EXOSCALE_APIKEY']
      cloudstack.secret_key = ENV['EXOSCALE_SECRETKEY']
      
      cloudstack.host = "api.exoscale.ch"
      cloudstack.path = "/compute"
      cloudstack.port = "443"
      cloudstack.scheme = "https"
      cloudstack.template_id = "60869641-ac24-4e8e-9e5d-f29534b44532"
      cloudstack.network_id = "00304a04-c7ea-4e77-a786-18bc64347bf7"
      cloudstack.zone_id = "1128bd56-b4d9-4ac6-a7b9-c715b187ce11"
      cloudstack.network_type = "Basic"
      
      
      cloudstack.display_name = "dtr"
      cloudstack.service_offering_name = "Tiny"
      cloudstack.security_group_names = ['default']
      cloudstack.keypair = "lrolaz@home"
      cloudstack.ssh_key = "~/.ssh/id_rsa"
      cloudstack.ssh_user = "root"
      override.vm.box = "dummy"
    end
  end
  
  
 
end
