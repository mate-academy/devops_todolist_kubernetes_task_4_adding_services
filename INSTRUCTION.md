First of all create a namespace if not yet created:
```
kubectl apply -f .infrastructure/todoapp-namespace.yml
```

Than start two instances of app simultaniously by:
```
kubectl apply -f .infrastructure/todoapp-pod.yml
```

The next step is to create an ip service to balance working load:
```
kubectl apply -f .infrastructure/ip-service.yml
```

Then start the port forwarding from our cluster using:
```
kubectl apply -f .infrastructure/port-service.yml
```

Now you can connect to our app via http://localhost:30008/