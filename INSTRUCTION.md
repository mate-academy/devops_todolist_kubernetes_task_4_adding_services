Testing the ToDo application using the busyboxplus:curl container:
1. Connect to the busybox pod: kubectl -n todoapp exec -it busybox -- sh
2. Inside the shell: curl http://<service-name>.<namespace>.svc.cluster.local
3. To exit the shell, enter the command "exit".

Testing the ToDo application using port-forward:
1. Run the following command to forward a local port to the app service: kubectl port-forward service/<service-name> <local-port>:<service-port> -n <namespace>
2. Once the command is running, you can access the ToDo app by opening: http://localhost:<local-port>
3. Keep the terminal open while testing. Press Ctrl + C to stop the forwarding when done.

Testing the ToDo application using a NodePort Service:
1. You can access the app on your machine using: http://localhost:30007
