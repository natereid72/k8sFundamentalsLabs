## Lab 1 - Explore Cluster Namespaces with kubectl

If using the VMware Enterprise PKS lab, you may need to refresh cluster credentials before performing the steps in these labs. Perform the following two steps before proceeding to the lab directions.

- `pks login -a pks.corp.local -u pksadmin -p VMware1! -k`
- `pks get-credentials my-cluster`

Begin lab exercise:

1. From CLI-VM, execute `kubectl get ns`
2. Execute `kubectl describe ns default`

You should find there are four namespaces and the default namespace has no resource quutas or limits set.

3. Execute `kubectl create ns lab`
4. Execute `kubectl get ns`

You should find there are now five namespaces. The previous four, plus the new namespace you just created.

This is the end of lab 1. You performed a basic kubectl get command and retrieved information about namespaces in the cluster. Then you created a new namespace and verified its creation.

The steps in this lab are the foundation of all interactions with a K8s cluster and its resources.
