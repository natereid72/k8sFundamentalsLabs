## Lab 6 - Persistent Storage

In this lab, you will modify the index.html of an nginx pod, verify it has been modified, recreate the pod, see that your changes were lost, 
configure persistent storage for the pod, repeat the steps and verify changes are no longer lost.

To this point, you have been creating pods without controllers. If the pod deis, nothing will attempt to recreate it. In this lab you will 
create a pod with a *Deployment* controller. 

You will learn more about Deployment controller in the next section covered. For now, you can just go with the understandign that a 
Deployment attempts to make sure the inteded state of a pod, or pods, is correct. If a pod dies, it will recreate it to keep the state 
correct.

#### Define a Storage Class

From the cli-vm:
1. Execute `kubectl apply -f ~/k8sFundamentalsLabs/yaml/default-sc.yaml`
2. Execute `kubectl get sc`

