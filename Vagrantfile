# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "puppetlabs/centos-6.6-64-nocm"
  config.vm.hostname = "centos"

  config.vm.box_check_update = false
  config.vm.synced_folder "saltstack/srv", "/srv"
  config.vm.synced_folder "saltstack/etc", "/etc/salt"
  
  ##  Personal config goes here.
  # - salt source code
  config.vm.synced_folder "~/work/code/salt/salt", "/root/salt"
  # - vim
  config.vm.provision "file", source: "~/.vimrc", destination: ".vimrc"
  config.vm.synced_folder "~/.vim", "/home/vagrant/.vim"


  config.vm.provision "shell" do |s|
    ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
    s.inline = <<-SHELL
      echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
    SHELL
  end

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
    vb.name = "centos"
  end

  config.vm.define "centos" do |i|
  end

  config.vm.provision "shell", inline: <<-SHELL
    sudo yum -y upgrade
    sudo yum install -y vim tmux git python-devel python-pip git-core virt-what gcc-c++ m2crypto
    sudo rpm -ivh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
    sudo yum install -y python-pip
    sudo yum clean all
    sudo pip install --upgrade pip
    sudo pip install pyzmq PyYAML pycrypto msgpack-python jinja2 psutil 
  SHELL
end
