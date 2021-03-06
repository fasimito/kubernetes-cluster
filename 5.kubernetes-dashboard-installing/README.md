# Kubernetes Dashboard Installing
[Dashboard](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/) is a web-based Kubernetes user interface.
you can get the basic functions from the [official website](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/)

it's easy to deploy the application, but, it's not so easy to get the WebUI. this is show you how to get your UI in hard.

## Deploying the Dashboard UI
The Dashboard UI is not deployed by default. To deploy it, run the following command:
> kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/aio/deploy/recommended/kubernetes-dashboard.yaml

if you do so, there default TYPE is Cluster IP, that means, you could not access the site without a proxy. so, we can do some change with the yml file. do it with the bellow command:

> wget https://raw.githubusercontent.com/kubernetes/dashboard/master/aio/deploy/recommended/kubernetes-dashboard.yaml

and then change it in the marked place:

![image](https://github.com/fasimito/kubernetes-cluster/blob/master/images/dashboard-modify-service.jpg)

then, prepare the required image fellow the [Use github-docker to make images](https://github.com/fasimito/kubernetes-cluster/tree/master/4.use-github-docker-make-images):
```
image: k8s.gcr.io/kubernetes-dashboard-amd64:v1.10.1
```

then execute the fellow command:
> kubectl apply -f kubernetes-dashboard.yaml

to start the kubernetes dashboard service.

## Check the Services

> kubectl get services --all-namespaces 

![image](https://github.com/fasimito/kubernetes-cluster/blob/master/images/dashboard-check-services.jpg)

> kubectl describe svc kubernetes-dashboard -n kube-system

![image](https://github.com/fasimito/kubernetes-cluster/blob/master/images/dashboard-get-services.jpg)

then access the url: https://192.168.116.129:30001/login from the browser.

You should face many new problem when you access the URL.

as for me, I try to access the url with IE and safari I can't get any result except different kinds of error.
finally, I choose the Firefox to get the result.

![image](https://github.com/fasimito/kubernetes-cluster/blob/master/images/k8s-dashboard-ui.jpg)

you need use the command:

> kubectl create -f admin-user.yaml

generate the token to access the url.

