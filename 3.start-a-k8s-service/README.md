# Start A Kubernetes Service

If you have got the success feature of kubernetes, It's time to start the first service.
Let's start a new nginx service.

## Check the cluster nodes

> kubectl get node

![image](https://github.com/fasimito/kubernetes-cluster/blob/master/images/all-pods-status.jpg)

## Start a nginx service
> kubectl run my-nginx --image=nginx --replicas=2 --port=80

## Expose the service to public
> kubectl expose deployment my-nginx --port=8080 --target-port=80 --external-ip=192.168.116.128

```
--port container's port 
--Container-port and target-port are two kinds of ports forwarded by the host. They can be designated at will or not.  
--external-ip Exposed IP addresses are usually public IP addresses. After executing that command, we can access them on the public network. But there is a problem here that this IP address must be the IP of the machine with k8s installed. If you can't access it with any IP, it also causes inconvenience to the application.  
--type=LoadBalancer 
```
## Check pods
> kubectl get po

![image](https://github.com/fasimito/kubernetes-cluster/blob/master/images/check-pos-status.jpg)

## Check Service
> kubectl get svc

![image](https://github.com/fasimito/kubernetes-cluster/blob/master/images/check-service-status.jpg)

