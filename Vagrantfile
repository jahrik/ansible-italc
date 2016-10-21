# -*- mode: ruby -*-
# vi: set ft=ruby :

require_relative './.vagrant/key_authorization.rb'

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "bento/ubuntu-14.04"

  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.memory = "1024"
  end

  config.vm.provision "shell", inline: "mkdir -p /root/.ssh"

  authorize_key_for_root config, 'fles/ssh/id_dsa.pub', 'files/ssh/id_rsa.pub'

  config.vm.provision "shell", inline: "apt-get update"
  config.vm.provision "shell", inline: "useradd -m -p raspberry -s /bin/bash -U -G users,sudo pi"

  {
    'master-vm'   => '192.168.33.11',
    'client-vm'   => '192.168.33.12',
  }.each do |short_name, ip|
    config.vm.define short_name do |host|
      host.vm.network 'private_network', ip: ip
      host.vm.hostname = "#{short_name}.dev"
    end
  end
end
