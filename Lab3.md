## Lab 3 - Create Pods

In this lab, you will create two pods. You will use the `kubectl run` command to directly create the first pod. Then you will use the same command with the `-o yaml` switch to create a pod manifest file, and then the `kubectl apply -f` command to create a second pod from the manifest file.

There will not be a great deal of explanation about the commands or methods. There are links in the slide deck for you to follow and learn more about the methods used here. For now, the expectation is to create two pods, while having some exposure to different methods of `kubectl` to work with K8s.

#### Create First Pod

1. From the cli-vm, execute `kubectl run pod1 --image=nginx --restart=Never`

The output should be "pod/pod1 created". Verify the pod is in the *creating* state and then enters *running* state.

2. Execute `kubectl get po pod1 -w`

Once the pod shows status of running, use `CTRL-C` to exit the watch loop. You have created a pod named *pod1* with the nginx image running as a container. The API server received your request via kubectl, created entries in the cluster etcd, contacted the scheduler with directions, the scheduler directed a selected worker node to download the image and load it. There were more steps, but those are the basic steps that took place.

#### Create Second Pod

Now, you'll create a pod from a manifest file. You could write the manifest file from scratch with a text editor, or you can use the `kubectl run` command with the `-o yaml` switch and pipe it into a file. This latter method is a quick way to create a 'skeleton' of a manifest file. For the second pod, we will use this method to create a template and then create pod from that manifest.

3. From the cli-vm, execute `kubectl run pod2 --image=nginx --restart=Never -o yaml > pod2.yaml`
4. Execute `cat pod2.yaml`

You'll see that the pod2.yaml file contains the output produced by the inclusion of the `-o yaml` switch. This has generated a manifest file that you can now use to create a pod with name 'pod2' that has the image 'nginx'.

5. Execute `kubectl apply -f pod2.yaml`
6. Execute `kubectl get po pod2 -w`

Once the pod shows status of running, use `CTRL-C` to exit the watch loop.

You have created two pods, both running an nginx container. This is the end of Lab 3.
