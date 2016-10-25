# -*- mode: ruby -*-
# vi: set ft=ruby :

require_relative './files/key_authorization.rb'

VAGRANTFILE_API_VERSION = '2'.freeze

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = 'bento/debian-8.2'

  config.vm.provider 'virtualbox' do |vb|
    vb.gui = true
    vb.memory = '1024'
  end

  config.vm.provision 'shell', inline: 'mkdir -p /root/.ssh'
  authorize_key_for_root config, '~/.ssh/id_dsa.pub', 'files/ssh/id_rsa.pub'

  config.vm.provision 'shell', inline: 'apt-get update'
  config.vm.provision 'shell', inline: 'id -u pi &>/dev/null || useradd -m -s /bin/bash -p raspberry -U -G users,sudo pi'
  config.vm.provision 'shell', inline: 'apt-get install xfce4 lightdm -y'
  # config.vm.provision 'shell', inline: "mkdir /etc/ansible && cat 'localhost ansible_connection=local' > /etc/ansible/hosts"

#  config.vm.provision 'ansible' do |ansible|
#    ansible.limit = 'all'
#    ansible.inventory_path = './vagrant.ini'
#    ansible.raw_arguments  = [
#    "--private-key=/files/ssh/id_rsa"
#    ]
#    ansible.playbook = 'site.yml'
#  end

  {
    'debian-01'   => '192.168.1.11',
    #'debian-02'   => '192.168.33.12',
  }.each do |short_name, ip|
    config.vm.define short_name do |host|
      host.vm.hostname = "#{short_name}.dev"
      # host.vm.network "public_network", bridge: "wlp3s0"
      host.vm.network "public_network", bridge: "wlp3s0", auto_config: false
      host.vm.provision "shell",
        run: "always",
        inline: "ifconfig eth1 #{ip} netmask 255.255.255.0 up"
      # host.vm.network 'private_network', ip: ip
    end
  end
end
