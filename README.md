# Production-like K8s in VirtualBox

### Prerequisites
- kubectl
- VirtualBox
- Vagrant
- Ansible

### Configure
Vagrantfile:

MASTERS = 1 only, more not supported now
```
MASTERS = 1
NODES = 2
```


### Start cluster
```
./up.sh
```

### Destroy cluster
```
./destroy.sh
```

### Get config
```
./get-kubeconfig.sh > kube
KUBECONFIG=./kube kubectl get nodes
```
