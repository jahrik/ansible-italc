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
#192.168.1.101 ansible_user=tom
#192.168.1.102 ansible_user=tom
#192.168.1.103 ansible_user=tom
```


## Run ansible against all servers

Install from deb package
```
ansible-playbook italc_from_deb.yml
```

Install from source package off github (v2)
```
ansible-playbook italc_from_source.yml
```

Only run --tags ssh (because you just updated the dsa keys)
```
ansible-playbook italc_from_deb.yml --tags dsa
```
