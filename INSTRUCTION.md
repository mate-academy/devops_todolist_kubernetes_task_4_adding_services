
## Deploy the application

```bash
kubectl apply -f .infrastructure/
```

## Test via buxybox

```bash
kubectl -n todoapp exec -it busybox -- sh
```

And then execute

```bash
curl http://todoapp-clusterip-svc:8080/api/health
```

You should get `Health OK`

## Test via port-forward

```bash
kubectl port-forward service/todoapp-clusterip-svc 8080:8080 -n todoapp
```
Now you should be able to access wedsite `http://localhost:8080/`

## Access via NodePort service

you shoild be able to access wedsite `http://localhost:30007/`