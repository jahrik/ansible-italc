# iTalc installation with ansible

(github.com/iTALC/italc)[https://github.com/iTALC/italc]

(italc.sourceforge.net)[http://italc.sourceforge.net/]

### iTalc

iTalc is commonly used in schools allowing a teacher to control student's computers.

iTalc is a tool for controlling other computers in several ways:
- Remote desktop (vnc, rdp)
- Share teacher's desktop to show demos
- Lock student's computers
- Send text messages
- Powering on and off remote computers

### Install ansible

Debian
```
sudo apt-get install ansible
```

Alternatively you can install ansible using pip.
```
sudo apt-get intsall python-pip
sudo pip install ansible
```

### Create keys in the files/dsa_keys directory for italc to use

```
ssh-keygen -t dsa -f files/dsa_keys/italc_dsa_key
```

### Update the inventory.ini file with the master and client IP addresses

ex:
```
[master]
pi ansible_host=192.168.11.146 ansible_user=pi
master-vm.dev ansible_host=192.168.33.11

[clients]
pi ansible_host=192.168.11.146 ansible_user=pi
client-vm.dev ansible_host=192.168.33.11
```

### Run ansible

```
ansible-playbook site.yml 
ansible-playbook -l master site.yml 
ansible-playbook -l clients site.yml 
```

## Tags

### Run setup to add your user's ssh key to authorized hosts and enable passwordless sudo
```
ansible-playbook site.yml --tags setup
```

### Only install packages
```
ansible-playbook site.yml --tags package
```

### Update ssh keys and nothing else
```
ansible-playbook site.yml --tags ssh
```

### Run service related tasks
```
ansible-playbook site.yml --tags service
```

### Vagrant lab

Build a master and client vm and run ansible playbook site.yml against them.
```
vagrant up
```

If you make changes and want to test them.
```
vagrant reload --provision
```

This will automatically add your local user's ssh keys.

You can ssh to the machine with:
```
eval $(ssh-agent)
ssh-add

ssh master-vm.dev
ssh client-vm.dev
```

After adding ssh keys to ssh-agent you can run ansible normally.
```
ansible-playbook -i vagrant.ini site.yml
```
