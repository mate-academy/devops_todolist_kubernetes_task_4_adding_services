For using amazing todo application through DNS in ClusterIP service follow instruction below

1. Create namespace for application

```
kubectl apply -f .infrastructure/namespace.yml
```

2. Update default config namespace for more comfortable running application

```
kubectl config set-context --current --namespace=todoapp
```

3. Create 2 pods for balancing traffic of application

```
kubectl apply -f .infrastructure/todoapp-pod.yml
```

4. Create pod with curl to test ClusterIP service

```
kubectl apply -f .infrastructure/curl.yml
```

5. Check for all successfully created pods

```
kubectl get pods -o wide
```

6. Create ClusterIP service

```
kubectl apply -f .infrastructure/clusterIp.yml
```

7. Check for successfully created ClusterIP service

```
kubectl get svc -o wide
```

8. Run interactive shell in curl pod by running command

```
kubectl exec -it curl -- sh
```

9. Check for successfully response after request inside cluster through ClusterIP service in curl container

```
curl http://todoapp-service.todoapp.svc.cluster.local
```

10. Run exit

```
exit
```

11. Also you could test application after running port-forwarding setup

```
kubectl port-forward service/todoapp-service 8081:80
```

Now you can enjoy fantastic todo application on http://localhost:8081/ or http://127.0.0.1:8081/
Run Ctrl+C to stop port-forwarding

12. Also for using NodePort service create it by running command

```
kubectl apply -f .infrastructure/nodeport.yml
```

13. Check for successfully created NodePort service

```
kubectl get svc -o wide
```

Now you can enjoy fantastic todo application on http://localhost:30007/ or http://127.0.0.1:30007/
