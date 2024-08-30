## ToDo app

1. set context
   run `kubectl config set-context --current --namespace=todoapp`

2. to run everything
   run `kubectl apply -f .infrastructure`

3. test via browser
   open `localhost:8080`

4. test via BB
   run `kubectl exec -it busybox -- sh`
   run `curl http://todoapp-service.todoapp.svc.cluster.local`

5. test via nodePort
   open `localhost:30007`
