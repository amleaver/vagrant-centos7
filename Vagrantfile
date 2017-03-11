# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "bento/centos-7.3"

  config.vm.provider "virtualbox" do |v|
    v.memory = 4096
    v.cpus = 2
    v.gui = true
    v.customize ["modifyvm", :id, "--vram", "32"]
    v.name = "centos7"
    v.customize ["modifyvm", :id, "--clipboard", "bidirectional"]
  end

  config.vm.network "private_network", ip: "192.168.2.100"

  config.vm.provision "shell", inline: <<-SHELL
    yum -y update
    yum -y install vim git wget sudo zsh tmux dnf ack
    yum -y install epel-release

    yum -y groupinstall "X Window system"
    yum -y groups install "MATE Desktop"
    #echo "exec /usr/bin/mate-session" >> ~/.xinitrc
    ln -sf /lib/systemd/system/graphical.target /etc/systemd/system/default.target
    systemctl isolate graphical.target

    systemctl disable firewalld

    wget https://copr.fedorainfracloud.org/coprs/thelocehiliosan/yadm/repo/epel-7/thelocehiliosan-yadm-epel-7.repo -P /etc/yum.repos.d/
    yum -y install yadm

    su - vagrant -c "gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3"
    su - vagrant -c "\\curl -sSL https://get.rvm.io | bash -s stable"
    su - vagrant -c "rvm install ruby-2.3"
    su - vagrant -c "rvm --default use 2.3"

    wget https://dl.google.com/linux/direct/google-chrome-stable_current_x86_64.rpm -P /tmp
  SHELL
end
