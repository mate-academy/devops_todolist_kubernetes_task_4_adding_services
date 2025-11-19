# INSTRUCTION.md

# How to Test ToDo Application in Kubernetes

This guide explains how to test the ToDo application deployed in Kubernetes using ClusterIP, NodePort and port-forward.

Namespace used: todoapp.
Services:
- todolist-clusterip (ClusterIP)
- todolist-nodeport (NodePort)

---

## 1. Test ToDo App via ClusterIP from BusyBox

Start BusyBox inside the same namespace:

kubectl run busybox --image=busybox:1.36 -it --rm --restart=Never -n todoapp -- sh

Inside BusyBox shell execute:

wget -O- http://todolist-clusterip:8080

If successful, BusyBox prints the HTML of the ToDo application.

---

## 2. Test ToDo App Using Port-Forward

Forward service port to local machine:

kubectl port-forward service/todolist-clusterip 8080:8080 -n todoapp

Open in browser:

http://localhost:8080

Or via curl:

curl http://localhost:8080

---

## 3. Access ToDo App Using NodePort Service

Get node IP address:

kubectl get nodes -o wide

Open the application in browser:

http://<NODE-IP>:30080

Or using curl:

curl http://<NODE-IP>:30080

You should receive ToDo app response.

---

## 4. Validate Load Balancing (Optional)

Check pods with the same label:

kubectl get pods -l app=todolist -n todoapp -o wide

You should see two pods:
- todoapp
- todoapp-reserv-pod

ClusterIP service load balances traffic between them.

---

## 5. Cleanup (Optional)

kubectl delete ns todoapp

---

# End of Instruction