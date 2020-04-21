# -*- mode: ruby -*-
# vi: set ft=ruby :

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
#  config.vm.box = "ubuntu"
#  config.vm.box = "ubuntu/trusty64"
#  config.vm.box = "ubuntu/xenial64"
  config.vm.box = "ubuntu/bionic64"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
# config.vm.synced_folder "../data", "/vagrant_data"
# config.vm.synced_folder "../awayTest", "/vagrant_data"
# config.vm.synced_folder "../awayTest", "/var/www/html"
  config.vm.synced_folder "../", "/var/www/html"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
config.vm.provision "shell", inline: <<-SHELL
  apt-get update
  
  apt-get install -y apache2
  apt-get install -y php
  apt-get install -y emacs

  ############
  # docker
  ############

  # SET UP THE REPOSITORY
  # 1
  apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common

  # 2
  curl -fsSL https://download.docker.com/linux/ubuntu/gpg > /tmp/___ubuntu_shinji_docker_pkg
  apt-key add /tmp/___ubuntu_shinji_docker_pkg

  # 3
  add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

  # INSTALL DOCKER ENGINE
  apt-get update -y
  apt-get install -y docker-ce docker-ce-cli containerd.io

  mkdir /dockerd
  echo 'sudo docker rm -f samba ; sudo docker run -it --name samba -p 139:139 -p 445:445 -v /dockerd:/dockerd -d dperson/samba -s "dockerd;/dockerd;yes;no"' > /dockerd/docker_samba.sh
  chmod +x /dockerd/docker_samba.sh
  
  echo '\nListen 8080\n' >> /etc/apache2/ports.conf


  echo  '<VirtualHost *:8080>\n\
        ServerAdmin webmaster@localhost\n\
        DocumentRoot /dockerd\n\
        ErrorLog ${APACHE_LOG_DIR}/error.log\n\
        CustomLog ${APACHE_LOG_DIR}/access.log combined\n\
        <Directory /dockerd/>\n\
          Options Indexes FollowSymLinks\n\
          AllowOverride None\n\
          Require all granted\n\
      	</Directory>\n\
  </VirtualHost>\n\
  ' > /etc/apache2/sites-available/001-docker.conf

  ln -s /etc/apache2/sites-available/001-docker.conf /etc/apache2/sites-enabled/001-docker.conf

  systemctl restart apache2

SHELL
end
