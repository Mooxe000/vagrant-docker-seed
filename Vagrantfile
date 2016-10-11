# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|

  hosts = {
    "NODE01" => "192.168.33.11",
    # "NODE01" => "192.168.33.12"
  }

  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  hosts.each do |name, ip|

    # Every Vagrant development environment requires a box. You can search for
    # boxes at https://atlas.hashicorp.com/search.

    config.vm.define name do |machine|

      # coreos-alpha
      # ubuntu/trusty64
      # ubuntu/xenial64
      machine.vm.box = "ubuntu/xenial64"
      machine.vm.hostname = name

      # Disable automatic box update checking. If you disable this, then
      # boxes will only be checked for updates when the user runs
      # `vagrant box outdated`. This is not recommended.
      machine.vm.box_check_update = false

      # Create a forwarded port mapping which allows access to a specific port
      # within the machine from a port on the host machine. In the example below,
      # accessing "localhost:8080" will access port 80 on the guest machine.
      # machine.vm.network "forwarded_port", guest: 80, host: 8080

      # Create a private network, which allows host-only access to the machine
      # using a specific IP.
      # machine.vm.network "private_network", ip: "192.168.33.10"
      # machine.vm.network :private_network, ip: ip
      machine.vm.network :private_network, ip: ip

      # Create a public network, which generally matched to bridged network.
      # Bridged networks make the machine appear as another physical device on
      # your network.
      # machine.vm.network "public_network"

      # Share an additional folder to the guest VM. The first argument is
      # the path on the host to the actual folder. The second argument is
      # the path on the guest to mount the folder. And the optional third
      # argument is a set of non-required options.
      machine.vm.synced_folder ".", "/home/ubuntu/vagrant"
      # machine.vm.synced_folder ".", "/home/core/vagrant"

      # Provider-specific configuration so you can fine-tune various
      # backing providers for Vagrant. These expose provider-specific options.
      # Example for VirtualBox:
      #
      machine.vm.provider "virtualbox" do |vb|

        # Display the VirtualBox GUI when booting the machine
        # vb.gui = true
        # vb.name = name

        # Customize the amount of memory on the VM:
        vb.memory = "3096"

        # vb.customize ["modifyvm", :id, "--memory", 512]

      end

      #
      # View the documentation for the provider you are using for more
      # information on available options.

      # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
      # such as FTP and Heroku are also available. See the documentation at
      # https://docs.vagrantup.com/v2/push/atlas.html for more information.
      # machine.push.define "atlas" do |push|
      #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
      # end

      # Enable provisioning with a shell script. Additional provisioners such as
      # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
      # documentation for more information about their specific syntax and use.

      machine.vm.provision "shell", inline: <<-SHELL

        # Update system
        cp /etc/apt/sources.list /etc/apt/sources.list.en
        cp /home/ubuntu/vagrant/sources/16.04.list /etc/apt/sources.list

        apt update && apt upgrade -y

        # Install docker
        curl -sSL https://get.daocloud.io/docker-experimental | sh
        # curl -sSL https://get.daocloud.io/docker | sh
        usermod -aG docker ubuntu

        # usr bin
        mkdir -p /home/ubuntu/bin

        # Install docker-compose
        curl -L https://get.daocloud.io/docker/compose/releases/download/1.8.1/docker-compose-`uname -s`-`uname -m` > /home/ubuntu/bin/docker-compose
        chown ubuntu:ubuntu /home/ubuntu/bin/docker-compose
        chmod +x /home/ubuntu/bin/docker-compose

        # docker speed tool
        # curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s http://c2095577.m.daocloud.io
        cp /home/ubuntu/vagrant/daemon.json /etc/docker/daemon.json
        systemctl restart docker.service

      SHELL

    end
  end
end
