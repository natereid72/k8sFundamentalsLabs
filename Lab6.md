## Lab 6 - Persistent Storage

In this lab, you will modify the index.html of an nginx pod, verify it has been modified, recreate the pod, see that your changes were lost, 
configure persistent storage for the pod, repeat the steps and verify changes are no longer lost.

To this point, you have been creating pods without controllers. If the pod deis, nothing will attempt to recreate it. In this lab you will 
create a pod with a *Deployment* controller. 

You will learn more about Deployment controller in the next section covered. For now, you can just go with the understandign that a 
Deployment attempts to make sure the inteded state of a pod, or pods, is correct. If a pod dies, it will recreate it to keep the state 
correct.

First, you'll create a default storage class for the cluster with an existing file. The contents of the file are this:

```
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  annotations:
    storageclass.kubernetes.io/is-default-class: "true"
  name: thin
parameters:
  diskformat: thin
provisioner: kubernetes.io/vsphere-volume
```

#### Create a Storage Class

From the cli-vm:
1. Execute `kubectl apply -f ~/k8sFundamentalsLabs/yaml/default-sc.yaml`
2. Execute `kubectl get sc`

You now have a storage class defined in the cluster that uses the in-tree vsphere-volume provider. You can now create persistent volume claims that will use that storage class by default to create and bimd to a persistent volume.

You will again use a file that already exusts, with the following contents:

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-claim
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 1Mi
```

#### Create a Persisten Volume Claim

From cli-vm:
1. Execute `kubectl apply -f ~/k8sFundamentalsLabs/yaml/pvc-1.yaml`
2. Execute `kubectl get pvc`
3. Execute `kubectl get pv`

Check with steps 2 and 3 until the PVC and PV have a status of *Bound*. You now have a persistent volume provisioned and a corresponding claim. The last step is to configure a pod to use the persistent volume by referenceing the claim name (my-claim). But first, let's see what happens when we don't have a persistent volume configured for a pod.

#### Create a Pod and Change a File

In this exercise, you will create an nginx pod, replace the default nginx index.html, delete the pod, recreate it, and see your changes lost.



