# -*- mode: ruby -*-
# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  # config.vm.box = "ubuntu/trusty64"
  config.vm.define "h01" do |h01|
    h01.vm.box = "ubuntu/trusty64"
    h01.vm.hostname = "h01"
    h01.vm.network "private_network", ip: "172.16.1.101"
    h01.vm.provision "file", source: "./h01.bird6.conf", destination: "/tmp/bird6.conf"
    h01.vm.provision "file", source: "./h01.docker-compose.yml", destination: "/tmp/comp/docker-compose.yml"
    h01.vm.provision "shell", inline: <<-SHELL
       # ipv6
       ip -6 addr add 2fff::1001/64 dev eth1
       # docker
       echo deb https://apt.dockerproject.org/repo ubuntu-trusty main > /etc/apt/sources.list.d/docker.list
       apt-key adv --keyserver keyserver.ubuntu.com --recv-keys F76221572C52609D
       apt-get update
       apt-get install -y docker-engine
       usermod -aGdocker vagrant
       # bird 
       apt-get install -y bird6
       cp /tmp/bird6.conf /etc/bird/bird6.conf
       service bird6 restart
       service docker restart
       # docker-compose
       curl -L https://github.com/docker/compose/releases/download/1.6.2/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
       chmod +x /usr/local/bin/docker-compose
       cd /tmp/comp/
       docker-compose up -d
       ip -6 a add dev comp-br0 2f00:1::1/64
     SHELL
  end
  config.vm.define "h02" do |h02|
    h02.vm.box = "ubuntu/trusty64"
    h02.vm.hostname = "h02"
    h02.vm.network "private_network", ip: "172.16.1.102"
    h02.vm.provision "file", source: "./h02.bird6.conf", destination: "/tmp/bird6.conf"
    h02.vm.provision "file", source: "./h02.docker-compose.yml", destination: "/tmp/comp/docker-compose.yml"
    h02.vm.provision "shell", inline: <<-SHELL
       # ipv6
       ip -6 addr add 2fff::1002/64 dev eth1
       # docker
       echo deb https://apt.dockerproject.org/repo ubuntu-trusty main > /etc/apt/sources.list.d/docker.list
       apt-key adv --keyserver keyserver.ubuntu.com --recv-keys F76221572C52609D
       apt-get update
       apt-get install -y docker-engine
       usermod -aGdocker vagrant
       # bird 
       apt-get install -y bird6
       cp /tmp/bird6.conf /etc/bird/bird6.conf
       service bird6 restart
       service docker restart
       # docker-compose
       curl -L https://github.com/docker/compose/releases/download/1.6.2/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
       chmod +x /usr/local/bin/docker-compose
       cd /tmp/comp/
       docker-compose up -d
       ip -6 a add dev comp-br0 2f00:2::1/64
     SHELL
  end
end
