# kubernetes Kafka Cluster Building

As is known that kafka is a good message queue product which had been widely applied in many corporations.

I would show you how to build a kafka cluster in kubernetes environment.

## 1. create a zookeeper cluster
kafka requires the zookeeper to do some synchonization. so, I need firstly create a zookeeper cluster.

> kubectl create -f zookeeper-svc.yaml

then execute:

> kubectl create -f zookeeper-deployment.yaml

after that, you need have a check the log if there is any error.

> kubectl log 


NOTES:

A very important command must execute in the kubernetes environments:
> kubectl create clusterrolebinding fixRBACKafka --clusterrole=cluster-admin --serviceaccount=kafka:default

please notes [here](https://github.com/Yolean/kubernetes-kafka/issues/232)