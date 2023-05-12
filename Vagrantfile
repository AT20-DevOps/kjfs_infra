# -*- mode: ruby -*-
# vi: set ft=ruby :

$script = <<-SCRIPT
  cd service
  docker-compose up -d
SCRIPT

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "ubuntu/bionic64"
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
  end

  config.vm.provision :docker
  config.vm.provision :docker_compose

  config.vm.define "server-1" do |serveremtpy|
    serveremtpy.vm.hostname = "ci-server"
    serveremtpy.vm.network "private_network", ip: "192.168.56.60"
  end

  config.vm.define "server-2" do |dockerserver|
    dockerserver.vm.hostname = "server-2"
    dockerserver.vm.network "private_network", ip: "192.168.56.61"
    dockerserver.vm.provision :file, source: "service/compiler_service", destination: "service/compiler_service"
    dockerserver.vm.provision :file, source: "service/docker-compose.yaml", destination: "service/docker-compose.yaml"
    dockerserver.vm.provision :file, source: "service/.env", destination: "service/.env"
    dockerserver.vm.provision :shell, inline: $script
  end


  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
end
