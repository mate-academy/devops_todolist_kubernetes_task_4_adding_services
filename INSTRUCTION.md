# Instructions

## Prerequisites

Before applying the Kubernetes manifests, make sure that the following tools are installed and configured:

Docker

kubectl

kind or another running Kubernetes cluster

A Docker Hub account

Access to the 

[ikulyk404/todoapp:3.0.0](https://hub.docker.com/r/ikulyk404/todoapp) image

Verify that the Kubernetes cluster is running and that kubectl can connect to it:

kubectl cluster-info

You can also verify the cluster nodes:

```bash
kubectl get nodes
```

And chack if you have running pod with busybox conteiner

```bash
kubectl get pods
```

# Testing

Before testing you need to go to dirrectory .infrastucture and run following commands:

```bash
kubectl apply -f todaapp-pod.yml
```

```bash
kubectl apply -f ClusterIp.yml
```

## Checking from busybox container

To check your app from busybox container run following commands:

```bash
kubectl -n todoapp exec -it busybox -- sh
```
Than inside this conteiner run

```bash
curl todoapp-service.todoapp.svc.cluster.local.
```

## Chesking with port-forward

run following command:

```bash
kubectl port-forward service/todoapp-service 8081:80
```
Now you can open [localhost:8081](localhost:8081) in your browser

## Acces with NodePort service

Run following command:

```bash
kubectl apply -f NodePort.yml
```
now you can write this adress in your browser and check your app

http://<NodeIP>:30008

(I have only one node so I used "localhost" as <NodeIP>)