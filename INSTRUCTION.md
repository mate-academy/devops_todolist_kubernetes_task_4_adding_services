# 1. Deploy BusyBox pod

kubectl run busybox --image=<image-name> --restart=Never -it -- /bin/sh

# 2. Inside the BusyBox shell, run:

nslookup <service-name>

# OR test the HTTP endpoint (if applicable):

wget -qO- http://<service-name>.<namespace>.svc.cluster.local

# Example:

wget -qO- http://my-service.default.svc.cluster.local

---

# 1. Port-forward the service to your local machine

kubectl port-forward service/todoapp 8080:80

# 2. In a new terminal or browser, access the app locally

curl http://localhost:8080

---

# 1. Get the IP address of any cluster node

kubectl get nodes -o wide

# 2. Get the NodePort assigned to the service

kubectl get service <service-name> -o jsonpath='{.spec.ports[0].nodePort}'

# 3. Access the app using Node IP and NodePort

curl http://<node-ip>:<node-port>
