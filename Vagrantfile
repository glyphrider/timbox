# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"

  config.vm.define "timbox" do |box|
    box.vm.provider "virtualbox" do |vb|
      vb.gui = true
      vb.name = "timbox"
      vb.memory = "4096"
    end
    box.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -yq ubuntu-desktop
      apt-get install -yq git build-essential libssl-dev libreadline-dev zlib1g-dev
    SHELL
    box.vm.provision "shell", privileged: false, inline: <<-USER_SHELL
      git clone https://github.com/rbenv/rbenv.git ~/.rbenv
      echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
      echo 'eval "$(rbenv init -)"' >> ~/.bashrc
      export PATH="$HOME/.rbenv/bin:$PATH"
      rbenv init
      mkdir -p "$(rbenv root)"/plugins
      git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build
      rbenv install 2.5.0
      rbenv global 2.5.0
      $HOME/.rbenv/shims/gem install bundler
      mkdir -p $HOME/.ssh
    USER_SHELL
    config.vm.provision :shell, privileged: false, inline: 'ssh-keyscan github.com >> ~/.ssh/known_hosts'
    key_files = Dir.glob(File.join(Dir.home,'.ssh','id_*')) + Dir.glob(File.join(Dir.home,'.ssh','*.pem'))
    key_files.each do |ssh_file|
      dest_file = File.join('.ssh',File.basename(ssh_file))
      config.vm.provision :file, source: ssh_file, destination: dest_file
    end
    config.vm.provision :shell, privileged: false, inline: 'chmod 400 ~/.ssh/*'
    config.vm.provision :shell, privileged: false, inline: 'chmod 600 ~/.ssh/*.pub'
  end
end
