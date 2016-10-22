# iTalc installation with ansible

## Install ansible

```
apt-get install ansible
```

## Create keys in the files/dsa_keys directory

```
ssh-keygen -t dsa -f files/dsa_keys/italc_dsa_key
```

## Update the inventory.ini file with the master and client IP addresses

ex:
```
[master]
192.168.11.146 ansible_user=pi

[clients]
192.168.11.146 ansible_user=pi

[vagrant]
#192.168.33.11 ansible_user=tom
#192.168.33.12 ansible_user=tom
```

## Run ansible

```
ansible-playbook site.yml 
ansible-playbook -l master site.yml 
ansible-playbook -l clients site.yml 
```

Update ssh keys and nothing else
```
ansible-playbook site.yml --tags ssh
