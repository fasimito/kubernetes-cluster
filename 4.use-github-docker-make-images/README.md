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

I have finished the accounts link, if you haven't, it's easy to fellow the steps.

#### 1.3 create repositories
find the "repositories" tab, click the "Create Repository" button, there should a new page,
add the name then finish a repository's create.

#### 1.3 build the repository
find a label named "builds" click the button "Configure Automated Builds" to choose the github repository and set build related parameters.
then you can get your own docker repository (docker image).
you can use docker pull command to download the image directly.
for me, I can use the next:
> docker push fasimito/k8sdashborad

to download my image, and then make a tag of the image like:
> docker tag fasimito/k8sdashborad k8s.gcr.io/kubernetes-dashboard-amd64:v1.10.1

so that, I get the k8s kubernetes-dashboard image.

