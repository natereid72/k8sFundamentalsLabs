## Lab 4 - Explore Pod and Node Networking

In this lab, you'll review cluster pod and node networking values. The node networking is essentially a product of how you define container host/k8s node IP assignment. Pod networking is a product of the CIDR you define to be used by K8s for pods. Depending on the 
method to install K8s and the CNI plugin you choose, this point of confiuration will vary. Irrespective of method, K8s will expect an 
IP CIDR scheme to be assigned to pod IP allocation.

1. From the cli-vm, execute `kubetctl get nodes -o wide`

You will see theat all nodes are on the same subnet. This subnet may, or may not, be routable on the network. What is important to K8s 
is that they can cammounicate with each other.

2. Execute `kubectl get po -o wide`

You will see that the pods have an addess on a different CIDR range. This should always be a non-routable IP range that only the pods 
make use of. We generally wouldn't access pods by their pod IPs. We will use service IPs (covered later). Pods are meant to be empheral (not 
relaible), so their IP is not something we should count on. 

This is the end of Lab 4. There are many aspects to K8s networking and we have just scratched the surface here. Follow the link in the 
slide deck to learn more.
