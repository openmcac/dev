# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.provider "virtualbox" do |vb|
    vb.gui = true
    vb.memory = "4096"
  end

  config.ssh.forward_agent = true

  config.vm.provision "file", source: "~/.ssh/id_rsa", destination: "~/.ssh/id_rsa"
  config.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "~/.ssh/id_rsa.pub"

  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update
    sudo apt-get install -y gnome-shell gnome-terminal firefox git
    curl -sL https://deb.nodesource.com/setup | sudo bash -
    sudo apt-get install -y nodejs
    sudo apt-get install -y build-essential
    sudo npm install -g npm
    sudo npm install -g bower
    sudo npm install -g ember-cli
    sudo npm install -g phantomjs
  SHELL

  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    mkdir ~/repos
    cd ~/repos
    git clone git@github.com:openmcac/mcac-js
  SHELL
end
