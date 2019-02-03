# Make Docker image from your github
for most time, we need make a docker image from our github repository. when you start the kubernetes, it's not easy to pull the docker images. so here, I offer a new way to get the images.

## 1. Prepare GitHub and Docker Account
It's easy to register a github account and a docker account.

#### 1.1 crete github repository
I have crete the repository: https://github.com/fasimito/k8s-docker-images

![image](https://github.com/fasimito/kubernetes-cluster/blob/master/images/my-git-repository.jpg)

the dockerfile is very simple:
```
FROM k8s.gcr.io/kubernetes-dashboard-amd64:v1.10.1
```
#### 1.2 link docker account and github account
please login in the [docker hub](https://cloud.docker.com) firstly, then find the "Linked Accounts" setting:

![image](https://github.com/fasimito/kubernetes-cluster/blob/master/images/docker-account-setting.jpg)




