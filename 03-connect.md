## Create kube config
SSH to the master node and execute
```
mkdir .kube
sudo cp /etc/kubernetes/admin.conf .kube/config
sudo chown -R $(whoami):$(whoami) .kube/config 
```

### Check nodes
```
$ kubectl get nodes
NAME    STATUS   ROLES    AGE   VERSION
node1   Ready    master   2d    v1.19.2
node2   Ready    master   2d    v1.19.2
node3   Ready    <none>   2d    v1.19.2
node4   Ready    <none>   2d    v1.19.2
node5   Ready    <none>   2d    v1.19.2
node6   Ready    <none>   2d    v1.19.2
node7   Ready    <none>   2d    v1.19.2
```
