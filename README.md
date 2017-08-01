Testing this tutorial:
https://crondev.com/kubernetes-installation-kubeadm/

```
vagrant up
```


when all machines is running ssh into each of them
```
vagrant ssh <master|agent|agent2>
```

Update each node to latest version and install K8s
```
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update && sudo apt-get install -y docker-engine kubelet kubeadm kubectl kubernetes-cni
```

On the master
```
sudo kubeadm init --apiserver-advertise-address 192.168.33.10 --pod-network-cidr 10.244.0.0/16 --token 8c2350.f55343444a6ffc46 --kubernetes-version v1.6.1
```

On the master Configure kubectl to access the cluster
```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

Now download and start Flannel network CNI RBAC:
```
curl -O https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel-rbac.yml
kubectl apply -f kube-flannel-rbac.yml
```

Flannel config:
```
curl -O https://gist.githubusercontent.com/komljen/d0bcf9e4ab93d5915b7ecf2b15021411/raw/b2c6a616588f90937736adf9ec086f220d731a5f/kube-flannel.yml
kubectl apply -f kube-flannel.yml
```

To join the agents join the cluster using the command:
```
sudo kubeadm join --token 8c2350.f55343444a6ffc46 192.168.33.10:6443 --skip-preflight-checks
```


