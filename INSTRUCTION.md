To create Pods, Services, and a Namespace, you should first run the command:
```
kubectl apply -f <fileanme>
```

Then, depending on the service type, you should test it in different ways:

**For ClusterIP:**

1. After running the Pods and applying all manifests, connect to one of the Pods:
```
kubectl -n <namespace> exec -it <pod> -- sh
```
2. Inside the container, run the following command:
```
curl http://<servicename>.<namespace>.svc.cluster.local
```
3. Afterwards, you can find this query in the Pod logs using:
```
kubectl logs <selector of pods>
```
This will confirm that traffic is reaching the backend Pods, and in the case of multiple replicas, may demonstrate load balancing.

**For NodePort:**

1. Open your browser and navigate to:
```
http://localhost:<NodePort>
```