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
6 | Wait for a long time to get the pods referred images |  | there should need a long time waiting, or fellow step 5
-----

