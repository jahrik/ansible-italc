# iTalc installation with ansible

### Install ansible

```
apt-get install ansible
```

### Create keys in the files/dsa_keys directory

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

### Only install packages
```
ansible-playbook site.yml --tags packages
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
