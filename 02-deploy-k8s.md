## Get Kubespray
git submodule init

## Install prerequisites
```
cd kubespray
sudo pip3 install -r requirements.txt
```
## Define a variable for the internal IP Address of all servers
```
declare -a IPS=(10.240.0.10 10.240.0.11 10.240.0.12 10.240.0.20 10.240.0.21 10.240.0.22 10.240.0.23)
```
## SSH keys
- Generate ssh keys
- Copy public keys to all servers above

## Generate host inventory
```
cp -rfp inventory/sample inventory/taube-cluster
CONFIG_FILE=inventory/taube-cluster/hosts.yaml python3 contrib/inventory_builder/inventory.py ${IPS[@]}
```

## Create cluster
```
ansible-playbook -i inventory/taube-cluster/hosts.yaml --key-file ~/.ssh/taube  --become --become-user=root -u taube cluster.yml
```
