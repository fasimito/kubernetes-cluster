# Install kubelet kubeadm kubectl
after the docker is prepared, the next step is start install the kubelet kubeadm and kubectl.

## Preparation
the preparation is very important, and should not ignored by each of the steps. or the strange errors make people crash.

serial No. | operation | comments
----- | ------ | -----
1 | Close the firewalld | firewalld would cause many ports could not be accessed
2 | Close the Selinux | Selinux would cause the right lost
3 | Close the swap | Close the fstab's swap row
-----

