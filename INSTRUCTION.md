 busybox + ClusterIP DNS: spin up a temporary busybox pod, then from inside it run wget http://<clusterip-service-name> to prove the Service correctly routes traffic inside the cluster.
busybox + ClusterIP DNS:

- Create a temporary busybox pod and curl the ClusterIP service DNS to verify routing:

	```bash
	kubectl run --rm -it busybox --image=busybox --restart=Never -- sh -c "wget -O- http://todolist-clusterip:8000"
	```

	If the service routes correctly you should see an HTTP response from the app.

port-forward:

- Forward the ClusterIP service to your localhost for quick testing:

	```bash
	kubectl port-forward service/todolist-clusterip 8000:8000
	# then open http://localhost:8000 in your browser
	```

NodePort:

- Find a cluster node IP and the NodePort, then open the app in your browser:

	```bash
	kubectl get nodes -o wide    # find a node external/internal IP
	kubectl get svc todolist-nodeport -o yaml    # nodePort is 30080 in these manifests
	# open http://<node-ip>:30080 in your browser
	```

Replace `<node-ip>` with one of the addresses from `kubectl get nodes -o wide`.
