## Lab 2 - Explore Running Pods

#### Reference Material
https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/

In this lab, you will use kubectl to view information about running pods.

1. From the cli-vm, execute `kubectl get pods`

You will see there are no pods running in the current namespace. When you use `kubectl`,
it executes in a contxt that includes your cluster, credentials, and namespace. This course will not cover contexts in detail, it's
enough to understand that your context is set to to a namespace that has no pods currently running.

2. Execute `kubectl get ns` again.

You will see there is a *kube-system* namespace. Now execute the `kubectl get` command with the `-n` switch to specify namespace to look in.

3. Execute `kubectl get po -n kube-system`

You should now see a number of pods that are running in the *kube-system* namespace. Notice we used the shorthand *po* for *pods*. 
There are occasions when you can shorten resource references that the API server will understand.

The default output will display a table with five columns. One of which is 'READY'. The READY column conveys the readiness assessment of 
the containers in the pod. This assessemnt is based on something called a *Readiness Probe*. Yo will see that the kube-dns pod has three 
containers, while the remainng pods have a single container defined.

This is the end of Lab 2.
