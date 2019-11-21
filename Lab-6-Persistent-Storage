## Lab 6 - Persistent Storage

In this lab, you will modify the index.html of an nginx pod, verify it has been modified, recreate the pod, see that your changes were lost, 
configure persistent storage for the pod, repeat the steps and verify changes are no longer lost.

First, you'll create a default storage class for the cluster with an existing file. The content of the file is this:

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
1. Execute `kubectl apply -f https://raw.githubusercontent.com/natereid72/k8sFundamentalsLabs/master/yaml/default-sc.yaml`
2. Execute `kubectl get sc`

You now have a storage class defined in the cluster that uses the in-tree vsphere-volume provider. You can now create persistent volume claims that will use that storage class by default to create and bind to a persistent volume.

You will again use a file that already exists, with the following contents:

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

#### Create a Persistent Volume Claim

From cli-vm:
1. Execute `kubectl apply -f https://raw.githubusercontent.com/natereid72/k8sFundamentalsLabs/master/yaml/pvc-1.yaml`
2. Execute `kubectl get pvc`
3. Execute `kubectl get pv`

Check with steps 2 and 3 until the PVC and PV have a status of *Bound*. You now have a persistent volume provisioned and a corresponding claim. The last step is to configure a pod to use the persistent volume by referencing the claim name (my-claim). But first, let's see what happens when we don't have a persistent volume configured for a pod.

#### Change a File on a Pod / Delete & Recreate Pod

In this exercise, you will use the pod2 nginx pod from earlier to replace the default nginx index.html, delete the pod, recreate it, and see your changes lost.

From the cli-vm:
1. Execute `kubectl get po pod2`

  Verify that the pod is still running. If not, execute `kubectl apply -f pod2.yaml`

2. From your web browser, make sure you can still access the default web page

If you forgot the IP address, execute `kubectl get svc` again.

3. Execute `kubectl exec -it pod2  -- bash -c "echo '<h1>hello world' > /usr/share/nginx/html/index.html"`
4. Now check your web page (you may need to use an incognito window to avoid retrieving a cached copy)
  
  You should see your landing page has been updates with the text *hello world*

5. Execute `kubectl delete po pod2`
6. Execute `kubectl apply -f pod2.yaml`

Use `kubectl get po pod2` to verify when the pod is *Running*.

7. Perform step 4 again

You should see that your changes were lost and the default index.html file has returned. This is because we have not defined persistent storage for the pod. In the next exercise, you will configure the pod for persistent storage and then perform the above steps again. This time, your changes will persist.

#### Configure Persistent Storage

To save you some time, I've included an updated pod2.yaml manifest in the lab file system. If you compare the manifest you have created on your system from earlier, you will see the addition of .spec.volumes and .spec.containers.volumeMounts.

The manifest refers to our persistent volume claim bt its name (my-claim) and specifies a volume name of html-storage. This is used inside the manifest only to link to the container volumeMounts section. Under containers array, you see the volumeMounts section. This section refers to the volumes section to link to the persistent volume by our claim. It also provides a mountPath that points to a directory inside our container. In this case, the directory where nginx retrieves its index.html file from.

With this configuration, any changes written to that directory will be saved in the persistent volume and persist beyond the life of the pod.

```
apiVersion: v1
kind: Pod
metadata:
  name: pod2
  labels:
    run: pod2
spec:
  volumes:
    - name: html-storage
      persistentVolumeClaim:
        claimName: my-claim
  containers:
    - name: pod2
      image: nginx
      volumeMounts:
        - mountPath: "/usr/share/nginx/html"
          name: html-storage
 ```

From the cli-vm:

1. Execute `kubectl delete po pod2`
2. Execute `kubectl apply -f https://raw.githubusercontent.com/natereid72/k8sFundamentalsLabs/master/yaml/pod2.yaml`
3. Use `kubectl get po pod2` to verify when the pod is Running status.
4. Execute `kubectl exec -it pod2  -- bash -c "echo '<h1>hello world' > /usr/share/nginx/html/index.html"`
5. Now check your web page (you may need to use an incognito window to avoid retrieving a cached copy)
  
  You should see your landing page has been updated with the text *hello world*

6. Execute `kubectl delete po pod2`
7. Execute `kubectl apply -f https://raw.githubusercontent.com/natereid72/k8sFundamentalsLabs/master/yaml/pod2.yaml`

Use `kubectl get po pod2` to verify when the pod is *Running*.

8. Perform step 4 again

This time, you should see that the changes you made to index.html persist.

This is the end of the lab.

