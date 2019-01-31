# Install kubelet kubeadm kubectl
after the docker is prepared, the next step is start install the kubelet kubeadm and kubectl.

## Preparation
the preparation is very important, and should not ignored by each of the steps. or the strange errors make people crash.

serial No. | operation | command | comments
----- | ------ | ----- | -----
1 | Close the firewalld | systemctl disable firewalld && systemctl stop firewalld | firewalld would cause many ports could not be accessed
2 | Close the Selinux | vim /etc/selinux/config | Selinux would cause the right lost
3 | Close the swap | vim /etc/fstab | Close the fstab's swap row
4 | Make sure the timestamp is synchronized | yum -y install ntpdate | Use the 'ntpdate' to synchronize the date of servers
5 | Prepare the docker images | see [How to make docker images](http://www.google.com) | prepare the kubenetes related docker images
6 | Wait for a long time to get the pods referred images downloading and creating the pods |  | there should need a long time waiting, or fellow step 5
-----

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
