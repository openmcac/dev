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

  config.vm.provision "file", source: "sshconfig", destination: "~/.ssh/config"

  config.vm.provision "file",
                      source: "~/.ssh/id_rsa",
                      destination: "~/.ssh/id_rsa"

  config.vm.provision "file",
                      source: "~/.ssh/id_rsa.pub",
                      destination: "~/.ssh/id_rsa.pub"

  config.vm.provision "file",
                      source: "~/dropbox/secrets/mcac/development.mcac.church.secrets.yml",
                      destination: "/tmp/development.mcac.church.secrets.yml"

  config.vm.provision "system_build_tools", type: "shell", inline: <<-SHELL
    sudo apt-get update
    sudo apt-get install -y gnome-shell gnome-terminal firefox git
    curl -sL https://deb.nodesource.com/setup | sudo bash -
    sudo apt-get install -y nodejs
    sudo apt-get install -y build-essential
  SHELL

  config.vm.provision "front_end_tools", type: "shell", inline: <<-SHELL
    sudo npm install -g npm
    sudo npm install -g bower
    sudo npm install -g ember-cli
    sudo npm install -g phantomjs
  SHELL

  config.vm.provision "user_build_tools", type: "shell", privileged: false, inline: <<-SHELL
    git clone https://github.com/sstephenson/rbenv.git ~/.rbenv
    echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
    echo 'eval "$(rbenv init -)"' >> ~/.bashrc
    git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
    sudo apt-get install -y libssl-dev libreadline-dev zlib1g-dev libsqlite3-dev
    source ~/.bashrc
  SHELL

  config.vm.provision "back_end_tools", type: "shell", privileged: false, inline: <<-SHELL
    mkdir -p ~/repos
    rm -Rf ~/repos/basechurch
    git clone git@github.com:openmcac/basechurch ~/repos/basechurch
    ~/.rbenv/bin/rbenv install `cat ~/repos/basechurch/.ruby-version`
  SHELL

  config.vm.provision "setup_basechurch", type: "shell", privileged: false, inline: <<-SHELL
    cp /tmp/development.mcac.church.secrets.yml ~/repos/basechurch/spec/test_app/config/development-secrets.yml
    cd ~/repos/basechurch
    ~/.rbenv/shims/gem install bundler
    ~/.rbenv/shims/bundle install --without staging
    ~/.rbenv/shims/bundle exec rake db:migrate
    cd spec/test_app
    ~/.rbenv/shims/bundle exec rake db:seed
  SHELL

  config.vm.provision "setup_mcac_js", type: "shell", privileged: false, inline: <<-SHELL
    rm -rf ~/repos/mcac-js
    mkdir -p ~/repos
    cd ~/repos
    git clone git@github.com:openmcac/mcac-js
    cd mcac-js
    # npm install
    # bower install
  SHELL
end
