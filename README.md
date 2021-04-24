# asyn
## Install kubernetes and docker on each node
``` bash
sudo sh install-k8s.sh
```

## Install NVIDIA driver on worker nodes
``` bash
sudo sh install-nvidia-driver.sh
```

## Initialize a k8s cluster
### On master node
``` bash
sudo kubeadm init --pod-network-cidr=10.244.0.0/16 --apiserver-advertise-address=<master_addr>
sudo sh init-k8s-cluster.sh
```
### On  worker nodes
This command appears when you run 'kubeadm init ……' on your master node. Copy it and run on each worker node
``` bash
sudo kubeadm join <master_addr>:6443 --token 9nzp6b.gguo --discovery-token-ca-cert-hash sha256:1b9a48db383b
``` 

## Install NVIDIA k8s-device plugin on worker nodes
#### Install NVIDIA k8s-device plugin on each worker node
``` bash
sudo sh install-k8s-device-plugin.sh
```
#### Edit the docker daemon config file which is usually present at `/etc/docker/daemon.json` on each worker node:
```json
{
    "default-runtime": "nvidia",
    "runtimes": {
        "nvidia": {
            "path": "/usr/bin/nvidia-container-runtime",
            "runtimeArgs": []
        }
    }
}
```
#### Enabling GPU support in k8s on master node
``` bash
kubectl create -f https://raw.githubusercontent.com/NVIDIA/k8s-device-plugin/v0.6.0/nvidia-device-plugin.yml
```
