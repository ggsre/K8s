### Docker vs K8s  
Docker  
- Containers can Node1 cannot communicate with containers on Node2
- All containers on a node share the host IP space & must coordinate which ports they use on that node  
- If a container must be replaced, any hard-coded IP addresses will break  
 
K8s  
- All containers can communicate with other containers without NAT   
- All nodes can communicate with other containers (and vice versa) without NAT   
- The IP that the container sees itself as is the IP that others see it as   

### Installation 
setenforce 0  
sed -i --follow-symlinks 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux  
modprobe br_netfilter  
echo '1' > /proc/sys/net/bridge/bridge-nf-call-iptables  
swapoff -a  
yum install -y yum-utils device-mapper-persistent-data lvm2  
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo  
yum install -y docker-ce  
sed -i '/^ExecStart/ s/$/ --exec-opt native.cgroupdriver=systemd/' /usr/lib/systemd/system/docker.service   
systemctl daemon-reload  
systemctl enable docker --now  
cat <<EOF > /etc/yum.repos.d/kubernetes.repo  
[kubernetes]  
name=Kubernetes  
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64  
enabled=1  
gpgcheck=0  
repo_gpgcheck=0  
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg  
   https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg  
EOF  
yum install -y kubelet kubeadm kubectl  
systemctl enable kubelet  
kubeadm init --pod-network-cidr=10.244.0.0/16 (only on master)  
mkdir -p $HOME/.kube  
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config  
sudo chown $(id -u):$(id -g) $HOME/.kube/config  
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml  
Run the join command on all nodes  
kubectl get nodes  
 
### Pods running on Master 
- coredns
- etcd
- kube-apiserver
- kube-controller-manager 
- kube-flannel 
- kube-proxy 
- kube-scheduler 

### Pods running on nodes 
- kube-flannel 
- kube-proxy

### Components of Master and Nodes 
Master 
- Etcd 
- API Server 
- Controller Manager (Node Controller, Replication Controller, Endpoints Controller, Service Accounts & Token Controller) 
Nodes
- Proxy 
- Kubelet 
- Container Runtime 

