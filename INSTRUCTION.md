INSTRUCTIONS FOR DEPLOYING AND TESTING THE TODO APPLICATION

This document provides step-by-step instructions on how to deploy 
and verify the ToDo application within a Kubernetes cluster.

------------------------------------------------------------------
1. DEPLOYING MANIFESTS
------------------------------------------------------------------

First, navigate to the directory containing the manifest files:
$ cd .infrastructure

Apply the manifests in the following order to ensure proper setup:

1.1 Create the Namespace:
    $ kubectl apply -f namespace.yml

1.2 Launch the main application Pod:
    $ kubectl apply -f todoapp-pod.yml

1.3 Launch the helper Pod for testing (ClusterIP.yml):
    $ kubectl apply -f ClusterIP.yml

1.4 Launch the helper Pod for testing (NodePort.yml):
    $ kubectl apply -f NodePort.yml

1.5 Launch the helper Pod for testing (busybox.yml):
    $ kubectl apply -f busybox.yml
    

------------------------------------------------------------------
2. VERIFYING THE DEPLOYMENT
------------------------------------------------------------------

Before testing, ensure all Pods are in the 'Running' state and '1/1 Ready':
    $ kubectl get pods -n todoapp

------------------------------------------------------------------
3. TESTING VIA BUSYBOX (CLUSTERIP & DNS)
------------------------------------------------------------------

To verify internal connectivity and DNS resolution using the ClusterIP service:

3.1 Enter the busybox container:
    $ kubectl exec -it busybox -n todoapp -- sh

3.2 Inside the busybox shell, test the DNS resolution:
    / # nslookup todoapp-service

3.3 Test the application response via the ClusterIP service name:
    / # wget -O- http://todoapp-service

3.4 Exit busybox:
    / # exit

------------------------------------------------------------------
4. TESTING VIA PORT-FORWARD
------------------------------------------------------------------

If you want to access the ClusterIP service directly from your local machine:

4.1 Establish a port-forward to the service:
    $ kubectl port-forward svc/todoapp-service -n todoapp 8082:80

4.2 Open your browser or use curl in a new terminal:
    $ curl http://localhost:8082

------------------------------------------------------------------
5. ACCESSING THE APPLICATION VIA NODEPORT
------------------------------------------------------------------

The NodePort service exposes the application on a specific port on all Cluster Nodes.

5.1 Identify your Node IP address:
    $ kubectl get nodes -o wide

5.2 Access the application using the Node IP and the assigned NodePort (30007):
    URL: http://<NODE_IP>:30007
