## How to create and run Cluster IP and test it with "port-forward ": 

1. Create/start container with app 
``` kubectl apply -f todoapp-pod.yml ```
2. Create/start container with Sercive (ClusterIP) 
``` kubectl apply -f clusterIP.yml ```

### To test it: 
``` kubectl port-forward service/cluster-ip -n meteapp 8081:80 ```

``` http://localhost:8081 ```

## How to create and run Node Port :

1. Create/start container with app 
``` kubectl apply -f todoapp-pod.yml ```

2. Create/start container with Sercive (ClusterIP) 
``` kubectl apply -f nodePort.yml ```

### To test it: 
``` http://localhost:30009 ```


## How to test an app by calling a ClusterIP service DNS from a busybox container

1. Access the container
``` k exec busybox -n todoapp  -it  -- sh ```
2. Run curl inside the container 
``` curl http://cluster-ip.meteapp.svc.cluster.local ```