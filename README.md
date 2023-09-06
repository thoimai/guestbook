# coding-project-Build and Deploy a Simple Guestbook App

## Review Criteria


Task 1: Updation of the Dockerfile. (5 points)

Task 2: The guestbook image being pushed to IBM Cloud Container Registry correctly. (1 point)

Task 3: Index page of the deployed Guestbook – v1 application. (2 points)

Task 4: Horizontal Pod Autoscaler creation. (1 point)

Task 5: The replicas in the Horizontal Pod Autoscaler being scaled correctly. (2 points)

Task 6: The Docker build and push commmands for updating the guestbook.(2 points)

Task 7: Deployment configuration for autoscaling. (1 point)

Task 8: Updated index page of the deployed Guestbook – v2 application after rollout of the deployment. (2 points)

Task 9: The revision history for the deployment after rollout of the deployment. (2 points)

Task 10: The udpated deployment after Rollback of the update. (2 points)


---
* command to see the history of deployment rollouts:

```log
[theia: guestbook]$ kubectl rollout history deployment/guestbook
deployment.apps/guestbook 
REVISION  CHANGE-CAUSE
1         <none>
2         <none>
```

*  command to see the details of Revision of the deployment rollout:

```log
[theia: guestbook]$ kubectl rollout history deployments guestbook --revision=2
deployment.apps/guestbook with revision #2
Pod Template:
  Labels:       app=guestbook
        pod-template-hash=569c45d59c
  Containers:
   guestbook:
    Image:      us.icr.io/sn-labs-u1513319/guestbook:v2
    Port:       3000/TCP
    Host Port:  0/TCP
    Limits:
      cpu:      5m
    Requests:
      cpu:      2m
    Environment:        <none>
    Mounts:     <none>
  Volumes:      <none>
```

* command to get the replica sets and observe the deployment which is being used now:
```log
[theia: guestbook]$ kubectl get rs
NAME                              DESIRED   CURRENT   READY   AGE
guestbook-569c45d59c              1         1         1       7m18s
guestbook-69bdc89c56              0         0         0       36m
openshift-web-console-8979686c9   2         2         2       49m
```

* command to undo the deploymnent and set it to Revision 1:
```log
[theia: guestbook]$ kubectl rollout undo deployment/guestbook --to-revision=1
deployment.apps/guestbook rolled back
```

* command to get the replica sets after the Rollout has been undone. The deployment being used would have changed as below:
```log
[theia: guestbook]$ kubectl get rs
NAME                              DESIRED   CURRENT   READY   AGE
guestbook-569c45d59c              1         1         1       7m58s
guestbook-69bdc89c56              1         1         0       37m
openshift-web-console-8979686c9   2         2         2       50m
```