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

## Check the pods' details
> kubectl describe po my-nginx

the command would list all the nginx pods details:

![image](https://github.com/fasimito/kubernetes-cluster/blob/master/images/pods-details.jpg)

as list in the picture, the container's IP is : 10.244.1.2, and the port is 80/TCP so the fellow command:

> curl http://10.244.1.2

would get the nginx welcome page. the nginx is start successfully.

## Check the Service's details
the fellow command:
> kubectl describe service/my-nginx

would get all details:

![image](https://github.com/fasimito/kubernetes-cluster/blob/master/images/check-service-details.jpg)

as show in the image, the service's IP is a virtual IP and mapped the port 8080, so, the virtual IP and port could be accessed, and it would redirect the request to the real container's IP address.

> curl http://10.109.184.145:8080  

would also get the nginx's welcome page. further more, the External IP is mapped to the real server's IP, so, if you want to access the nginx from the Explore, you need access the external IP. this ip is the server's real IP exposed to the public.

![image](https://github.com/fasimito/kubernetes-cluster/blob/master/images/access-from-ie.jpg)
