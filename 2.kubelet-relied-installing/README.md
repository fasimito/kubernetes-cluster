# Install kubelet kubeadm kubectl
after the docker is prepared, the next step is start install the kubelet kubeadm and kubectl.

## Preparation
the preparation is very important, and should not ignored by each of the steps. or the strange errors make people crash.

serial No. | operation | command | comments
----- | ------ | ----- | -----
1 | Close the firewalld | systemctl disable firewalld && systemctl stop firewalld | firewalld would cause many ports could not be accessed
2 | Close the Selinux | vim /etc/selinux/config | Selinux would cause the right lost
3 | Close the swap | vim /etc/fstab | Close the fstab's swap row
4 | Set the "net.bridge.bridge" | vim /etc/sysctl.d/k8s.conf | add net.bridge.bridge-nf-call-ip6tables = 1 and net.bridge.bridge-nf-call-iptables = 1, then execute sysctl -p /etc/sysctl.d/k8s.conf
5 | Set the cgroupfs | docker info && cat /etc/systemd/system/kuberlet.service.d/10-kubeadm.conf | make sure the both group value equally.
6 | Make sure the timestamp is synchronized | yum -y install ntpdate | Use the 'ntpdate' to synchronize the date of servers
7 | Prepare the docker images | see [How to make docker images](http://www.google.com) | prepare the kubenetes related docker images
8 | Wait for a long time to get the pods referred images downloading and creating the pods |  | there should need a long time waiting, or fellow step 5
9 | Set the hostname and config the hosts file | hostnamectl set-hostname [server-name] | then config the /etc/hosts file, all servers' hostname must not equally, it would cause conflict.
-----

the step 4: set "net.bridge.bridge" would face the next error, and the solutions are next:

ERROR | SOLUTION
--- | ---
error: "net.bridge.bridge-nf-call-ip6tables" is an unknow key | sudo modprobe bridge && sudo ismod grep bridge && sodo sysctl -p /etc/sysctl.d/k8s.conf
sysctl: cannot stat /proc/sys/net/bridge/bridge-nf-call-ip6tables: No such file or folder | modprobe br_netfilter && ls /proc/sys/net/bridge && sudo sysctl -p /etc/sysctl.d/k8s.conf
------

## Install kubelet, kubeadm and kubectl
please make sure the preparation had been finished successfully, or, there are many kinds of errors.

### 1. Set the kubelet yum resources
> vim /etc/yum.repos.d/kubernetes.repo
```
[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=0
```
### 2. Install kubelet kubeadm kubectl
> yum  install -y kubelet kubeadm kubectl

### 3. Start the kubelet
> systemctl enable kubelet.service

### 4. Close Swap
> swapoff -a

## Create a cluster with kubeadm
up till now, the basic running environment had been installed, it's time to establish a kubernetes cluster.
as for me, I have downloaded the images firstly.
```
docker pull mirrorgooglecontainers/kube-apiserver:v1.13.2
docker pull mirrorgooglecontainers/kube-controller-manager:v1.13.2
docker pull mirrorgooglecontainers/kube-scheduler:v1.13.2
docker pull mirrorgooglecontainers/kube-proxy:v1.13.2
docker pull mirrorgooglecontainers/pause:3.1
docker pull mirrorgooglecontainers/etcd:3.2.24
docker pull coredns/coredns:1.2.6
```
then add a tag for them
```
docker tag docker.io/mirrorgooglecontainers/kube-apiserver:v1.13.2 k8s.gcr.io/kube-apiserver:v1.13.2
docker tag docker.io/mirrorgooglecontainers/kube-controller-manager:v1.13.2 k8s.gcr.io/kube-controller-manager:v1.13.2
docker tag docker.io/mirrorgooglecontainers/kube-scheduler:v1.13.2 k8s.gcr.io/kube-scheduler:v1.13.2
docker tag docker.io/mirrorgooglecontainers/kube-proxy:v1.13.2 k8s.gcr.io/kube-proxy:v1.13.2
docker tag docker.io/mirrorgooglecontainers/pause:3.1 k8s.gcr.io/pause:3.1
docker tag docker.io/mirrorgooglecontainers/etcd:3.2.24 k8s.gcr.io/etcd:3.2.24
docker tag docker.io/coredns/coredns:1.2.6 k8s.gcr.io/coredns:1.2.6
```
plan a reasonable network segment and initial a cluster with kubeadm
> kubeadm init --apiserver-advertise-address 192.168.116.128 --pod-network-cidr=10.244.0.0/16

Notes:
```
--apiserver-advertise-address string
The IP address the API Server will advertise it's listening on. Specify '0.0.0.0' to use the address of the default network interface.
-pod-network-cidr string
Specify range of IP addresses for the pod network. If set, the control plane will automatically allocate CIDRs for every node.
```
if you have got a success tip, I should give you a congratulation. or, please fellow the error and resolve it by lucky and courage.

## Config kubectl
```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
then add the flanneld network on master
> kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

then add all nodes into the cluster with the command tip of init
> kubeadm join 192.168.116.128:6443 --token 6joyjx.mny5wqyn3jgrpyzl --discovery-token-ca-cert-hash sha256:1daa67ed061733bb7e6aaac155870519b1512f49b7a20b84bbcc076e59198ebd

then, you can check the cluster status with the command on the master node:
> kubectl get nodes

you can check the cluster status.
run the fellow commands for further more system requiring.
```
# yum install -y bash-completion
# find / -name "bash_completion"
/usr/share/bash-completion/bash_completion
# source /usr/share/bash-completion/bash_completion
# source <(kubectl completion bash)
```
check all the pods with command:
> kubectl get pod --all-namespaces

wait for the images downloading. times later, you should found all the pods running.

![image](https://github.com/fasimito/kubernetes-cluster/blob/master/images/all-pods-status.jpg)

