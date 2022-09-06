## ==> ON BOTH NODES
```
- sudo apt update -y && sudo apt upgrade -y
- swapoff -a
- sudo reboot -f
- swapoff -a
```
```
swapoff -a                 # Disable all devices marked as swap in /etc/fstab
sed -e '/swap/ s/^#*/#/' -i /etc/fstab   # Comment the correct mounting point
systemctl mask swap.target               # Completely disabled
```
### Installing Docker packages:
```
apt install docker.io -y
```
```
sudo sudo systemctl start docker
sudo sudo systemctl enable docker
```
### Installing kubeadm, kubelet and kubectl:
```
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl
sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl kubernetes-cni
sudo apt-mark hold kubelet kubeadm kubectl kubernetes-cni
sudo apt update
```
- sudo systemctl daemon-reload
## ==> ONLY ON MASTER NODE
- sudo kubeadm init --pod-network-cidr=192.168.0.0/24

- kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"

## ==> ONLY ON WORKER NODE
- kubeadm join 192.168.6.128:6443 --token p0p5d0.w75nx4n8xg6ku5te \
        --discovery-token-ca-cert-hash sha256:7c84726ff5039d5caa6a347e508e7f04ddcf483153010341b0e5ac179b318ef1

- kubectl label node ip-172-31-38-105 node-role.kubernetes.io/worker=worker

## ==> To Downgrade the K8S version
```
 sudo apt remove kubelet kubeadm kubectl
 sudo kubectl version
 sudo apt install kubelet=1.23.4-00
 sudo apt install kubeadm=1.23.4-00
 sudo apt install kubectl=1.23.4-00
 sudo kubectl version
 ```
