## Lab 5 - Create Services and Access Pods

In this exercise, you will create cluster service objects that front-end a pod. Per the lecture, pods are ephemeral and their IP addresses 
are not consistent over their lifetime. For this reason, we create a service object that will act as a cluster load balancer to a pod(s) for other 
other pods to reach it.

This is known as a *clusterIP* address and will draw from a dfferent CIDR range than the pod IP range.

#### Create a ClusterIP Service

1. From the cli-vm, execute `kubectl get po pod1` to verify your pod pod1 is still running.
2. Execute `kubectl expose po pod1 --name=pod1-svc --port=8080 --target-port=80`
3. Execute `kubectl get svc`

You will see that you've created a service called *pod1-svc*, of type *ClusterIP*, with a cluster IP address in the CIDR range of 
10.100.200.0/24, no external-IP, and listening on port 8080/TCP.

ClusterIP is the default type when no type is specified during creation. Later, you will perform a similar service ceration but will 
specify *Type* to change this default behavior.

Now, try to access your pod on port 8080 from the command line of your cli-vm. Since the pod was created with an nginx container, you 
should be able to use the `curl` command to retrieve the default index.html page.

Make note of your services *ClusterIP* address (Write it down for future reference) and curl it.

4. Execute `curl 10.100.200.x:8080`  - Where *x* is your specific address

You will not retrieve the index.html with the above. Use `CTRL-C` to cancel the failing curl attempts.

The reason this does not work is bcause the pod and service IP addresses are non-routable, and not inteded to be used outside of the 
K8s cluster. These addresses are for K8s services to work with each other only.

So, how do you see if the container is working within the cluster scope? There are a couple of ways, we can use a specialy kubectl port 
forward method that creates a temporary proxy into the cluster. Or we can get an interactive shell within a pod and then execute 
our curl command from there. 

In the next step, you will be creating a temporary pod and executing curl from within the pod. 

5. Execute `kubectl run temp --image=nginx --restart=Never --rm -ti -- sh`
6. Wait for the `#` prompt to appear. You may need to hit the <Enter> key once or twice
7. Execute `apt update`
8. Execute `apt install curl -y`
9. Execute `curl <your-svc-ip-addr>:8080`

You should now succesfully retrieve the nginx default index.html file.

10. Execute `exit`

You are now back at the cli-vm shell and the temp pod has been deleted.

#### Create a LoadBalancer Service to Access Pod from Outside World

We've seen how to use a ClusterIP service to make a pod available inside the cluster, but what if we wanted to access the pod 
from outside of the cluster? For that, we have a few options. You'll use a service type *LoadBalancer* in this exercise.

1. Execute `kubectl expose po pod1 --type=LoadBalancer --port=8080 --target-port=80`
2. Execute `kubectl get svc`

Locate your service of type LoadBalancer and make note of the external IP address that begins with 10.40.14.x. From your web browser, 
go to `http://<external-ip-address>:8080`

You should now be able to access the nginx index.html from outside of the cluster.

This is the end of the lab.
