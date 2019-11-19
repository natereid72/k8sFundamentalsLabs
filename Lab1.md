Lab 1 - Explore Cluster Namespaces with kubectl
------

1. From CLI-VM, execute `kubectl get ns`
2. Execute `kubectl describe ns default`

You should find there are four namespaces and the default namespace has no resource quutas or limits set.

3. Execute `kubectl create ns lab`
4. Execute `kubectl get ns`

You should find there are now five namespaces. The previous four, plus the new namespace you just created.

This is the end of lab 1. You performed a basic kubectl get command and retrieved information about namespaces in the cluster. then you created a new namespace and verified its creation.
