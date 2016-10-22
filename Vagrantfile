# -*- mode: ruby -*-
# vi: set ft=ruby :

require_relative './.vagrant/key_authorization.rb'

VAGRANTFILE_API_VERSION = '2'.freeze

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = 'bento/debian-8.2'

  config.vm.provider 'virtualbox' do |vb|
    vb.gui = false
    vb.memory = '1024'
  end

  config.vm.provision 'shell', inline: 'mkdir -p /root/.ssh'
  authorize_key_for_root config, '~/.ssh/id_dsa.pub', 'files/ssh/id_rsa.pub'

  config.vm.provision 'shell', inline: 'apt-get update'
  config.vm.provision 'shell', inline: 'id -u pi &>/dev/null || useradd -m -p raspberry -s /bin/bash -U -G users,sudo pi'
  # config.vm.provision 'shell', inline: "mkdir /etc/ansible && cat 'localhost ansible_connection=local' > /etc/ansible/hosts"

  config.vm.provision 'ansible' do |ansible|
    ansible.limit = "all"
    ansible.inventory_path = './inventory.ini'
    ansible.raw_arguments  = [
    "--private-key=/files/ssh/id_rsa"
    ]
    ansible.playbook = 'site.yml'
  end

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
