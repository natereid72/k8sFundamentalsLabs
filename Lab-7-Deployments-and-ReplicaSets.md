## Lab 7 - Deployments and ReplicaSets

#### Reference Material
https://kubernetes.io/docs/concepts/workloads/controllers/deployment/

In this lab, you'll create a Deployment, scale the deployment, update the deployment with a new container image, and then perform 
a rollback. This is the foundation of managing scaled applications within a K8s cluster.

From the cli-vm:
1. Execute `kubectl run dep1 --image=natereid/webapp:1.0.0 --restart=Always --replicas=2`
2. Execute `kubectl get deploy dep1`

Perform the above command until you see *AVAILABLE* showing *2*

3. Execute `kubectl get po`

Notice there are 2 new pods, that begin with *dep1*, followed by some random text. These are the two pods created based on *--replicas=2* 
that you specified in step 1.

4. Execute `kubectl scale deploy dep1 --replicas=3`
5. Execute `kubectl get po`

Notice there are now 3 pods for this deployment.

6. Execute `kubectl expose deploy dep1 --type=LoadBalancer --port=80 --target-port=80`
7. Execute `kubectl get svc` and make note of the external 10.40.14.x IP address if the dep1 service.
8. Use your web browser to go to that IP address

You'll see the Application v1 web page with blue background. Next, you'll update the deployment with a new image. This is an example of 
how you would update an application. You'll see how the deployment keeps track of the changes.

9. Execute `kubectl set image deployment/dep1 dep1=natereid/webapp:2.0.0 --record`
10. Execute `kubectl rollour history deploy dep1`

You will see two revisions now, with the command you executed under change status for revisio 2. This is the result of using the --record 
switch.

Return to your web page, and you see it is now updated with the Application v2 and green background page. The rollout updated the ReplicaSet 
pods without outage.

11. Execute `kubectl rollout undo deploy dep1`

Check your web page again. You will eventually see that it returns to Application v1. In step 11, you rolled back the update.

This is the end of the lab.
