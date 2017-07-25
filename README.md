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
