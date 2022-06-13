# PFSwCO_NS-lab4

P.13.1

1.
```
docker buildx build . -t jakubartlomiej/lab4:nginx1 --platform linux/amd64 --push
docker buildx build . -t jakubartlomiej/lab4:nginx2 --platform linux/amd64 --push
```
2.
```
student@LabVM:~/Downloads/lab4/lab4$ kubectl apply -f nginx-deployment.yaml 
deployment.apps/nginx-deployment created
student@LabVM:~/Downloads/lab4/lab4$ kubectl get deployments
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   3/3     3            3           5m7s
student@LabVM:~/Downloads/lab4/lab4$ kubectl get pods
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-5f4847564d-dbpbf   1/1     Running   0          6m55s
nginx-deployment-5f4847564d-ppcvk   1/1     Running   0          6m55s
nginx-deployment-5f4847564d-z8tgf   1/1     Running   0          6m55s
student@LabVM:~/Downloads/lab4/lab4$ 
```
3.
```
student@LabVM:~/Downloads/lab4/lab4$ kubectl apply -f nginx-deployment.yaml 
deployment.apps/nginx-deployment configured
student@LabVM:~/Downloads/lab4/lab4$ kubectl get pods
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-5f4847564d-dbpbf   1/1     Running   0          8m4s
nginx-deployment-5f4847564d-jt6kr   1/1     Running   0          9s
nginx-deployment-5f4847564d-ppcvk   1/1     Running   0          8m4s
nginx-deployment-5f4847564d-s9w4x   1/1     Running   0          9s
nginx-deployment-5f4847564d-xd2l7   1/1     Running   0          9s
nginx-deployment-5f4847564d-z8tgf   1/1     Running   0          8m4s
student@LabVM:~/Downloads/lab4/lab4$ 
```
4.
```
student@LabVM:~/Downloads/lab4/lab4$ kubectl set image deployment/nginx-deployment nginx=jakubartlomiej/lab4:nginx2
deployment.apps/nginx-deployment image updated
student@LabVM:~/Downloads/lab4/lab4$ kubectl rollout status deployment/nginx-deployment
deployment "nginx-deployment" successfully rolled out
student@LabVM:~/Downloads/lab4/lab4$ kubectl describe deployment
Name:                   nginx-deployment
Namespace:              default
CreationTimestamp:      Mon, 13 Jun 2022 22:01:53 +0200
Labels:                 <none>
Annotations:            deployment.kubernetes.io/revision: 3
                        kubernetes.io/change-cause:
                          kubectl set resources deployment/nginx-deployment --containers=nginx --limits=cpu=300m,memory=1024Mi --record=true
Selector:               app=nginx
Replicas:               6 desired | 6 updated | 8 total | 5 available | 3 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=nginx
  Containers:
   nginx:
    Image:      jakubartlomiej/lab4:nginx2
    Port:       80/TCP
    Host Port:  0/TCP
    Limits:
      cpu:        300m
      memory:     1Gi
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    ReplicaSetUpdated
OldReplicaSets:  nginx-deployment-65b5678957 (2/2 replicas created)
NewReplicaSet:   nginx-deployment-67cd6d5bcb (6/6 replicas created)
Events:
  Type    Reason             Age                   From                   Message
  ----    ------             ----                  ----                   -------
  Normal  ScalingReplicaSet  4m13s                 deployment-controller  Scaled up replica set nginx-deployment-5f4847564d to 3
  Normal  ScalingReplicaSet  4m2s                  deployment-controller  Scaled up replica set nginx-deployment-5f4847564d to 6
  Normal  ScalingReplicaSet  3m18s                 deployment-controller  Scaled up replica set nginx-deployment-65b5678957 to 2
  Normal  ScalingReplicaSet  3m18s                 deployment-controller  Scaled down replica set nginx-deployment-5f4847564d to 5
  Normal  ScalingReplicaSet  3m18s                 deployment-controller  Scaled up replica set nginx-deployment-65b5678957 to 3
  Normal  ScalingReplicaSet  3m17s                 deployment-controller  Scaled down replica set nginx-deployment-5f4847564d to 4
  Normal  ScalingReplicaSet  3m17s                 deployment-controller  Scaled up replica set nginx-deployment-65b5678957 to 4
  Normal  ScalingReplicaSet  3m17s                 deployment-controller  Scaled down replica set nginx-deployment-5f4847564d to 3
  Normal  ScalingReplicaSet  3m17s                 deployment-controller  Scaled up replica set nginx-deployment-65b5678957 to 5
  Normal  ScalingReplicaSet  29s (x13 over 3m14s)  deployment-controller  (combined from similar events): Scaled up replica set nginx-deployment-67cd6d5bcb to
```
5.
```
student@LabVM:~/Downloads/lab4/lab4$ kubectl set resources deployment/nginx-deployment -c=nginx --limits=cpu=300m,memory=1024Mi
deployment.apps/nginx-deployment resource requirements updated
```
6.
```
student@LabVM:~/Downloads/lab4/lab4$ kubectl rollout history deployment/nginx-deployment
deployment.apps/nginx-deployment 
REVISION  CHANGE-CAUSE
1         <none>
2         kubectl set image deployment/nginx-deployment nginx=jakubartlomiej/lab4:nginx2 --record=true
3         kubectl set resources deployment/nginx-deployment --containers=nginx --limits=cpu=300m,memory=1024Mi --record=true

student@LabVM:~/Downloads/lab4/lab4$ kubectl rollout undo deployment/nginx-deployment --to-revision=1
deployment.apps/nginx-deployment rolled back
student@LabVM:~/Downloads/lab4/lab4$ kubectl rollout history deployment/nginx-deployment
deployment.apps/nginx-deployment 
REVISION  CHANGE-CAUSE
2         kubectl set image deployment/nginx-deployment nginx=jakubartlomiej/lab4:nginx2 --record=true
3         kubectl set resources deployment/nginx-deployment --containers=nginx --limits=cpu=300m,memory=1024Mi --record=true
4         <none>
student@LabVM:~/Downloads/lab4/lab4$ kubectl rollout history deployment/nginx-deployment
deployment.apps/nginx-deployment 
REVISION  CHANGE-CAUSE
1         <none>
2         kubectl set image deployment/nginx-deployment nginx=jakubartlomiej/lab4:nginx2 --record=true
3         kubectl set resources deployment/nginx-deployment --containers=nginx --limits=cpu=300m,memory=1024Mi --record=true

student@LabVM:~/Downloads/lab4/lab4$ kubectl rollout undo deployment/nginx-deployment --to-revision=1
deployment.apps/nginx-deployment rolled back
student@LabVM:~/Downloads/lab4/lab4$ kubectl rollout history deployment/nginx-deployment
deployment.apps/nginx-deployment 
REVISION  CHANGE-CAUSE
2         kubectl set image deployment/nginx-deployment nginx=jakubartlomiej/lab4:nginx2 --record=true
3         kubectl set resources deployment/nginx-deployment --containers=nginx --limits=cpu=300m,memory=1024Mi --record=true
4         <none>
student@LabVM:~/Downloads/lab4/lab4$ kubectl describe deployment
Name:                   nginx-deployment
Namespace:              default
CreationTimestamp:      Mon, 13 Jun 2022 22:01:53 +0200
Labels:                 <none>
Annotations:            deployment.kubernetes.io/revision: 4
Selector:               app=nginx
Replicas:               6 desired | 6 updated | 6 total | 6 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=nginx
  Containers:
   nginx:
    Image:        jakubartlomiej/lab4:nginx1
    Port:         80/TCP
    Host Port:    0/TCP
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   nginx-deployment-5f4847564d (6/6 replicas created)
Events:
  Type    Reason             Age                 From                   Message
  ----    ------             ----                ----                   -------
  Normal  ScalingReplicaSet  17m                 deployment-controller  Scaled up replica set nginx-deployment-5f4847564d to 6
  Normal  ScalingReplicaSet  16m                 deployment-controller  Scaled up replica set nginx-deployment-65b5678957 to 2
  Normal  ScalingReplicaSet  16m                 deployment-controller  Scaled down replica set nginx-deployment-5f4847564d to 5
  Normal  ScalingReplicaSet  16m                 deployment-controller  Scaled up replica set nginx-deployment-65b5678957 to 3
  Normal  ScalingReplicaSet  16m                 deployment-controller  Scaled up replica set nginx-deployment-65b5678957 to 4
  Normal  ScalingReplicaSet  16m                 deployment-controller  Scaled down replica set nginx-deployment-5f4847564d to 4
  Normal  ScalingReplicaSet  16m                 deployment-controller  Scaled down replica set nginx-deployment-5f4847564d to 3
  Normal  ScalingReplicaSet  16m                 deployment-controller  Scaled up replica set nginx-deployment-65b5678957 to 5
  Normal  ScalingReplicaSet  13m (x13 over 16m)  deployment-controller  (combined from similar events): Scaled up replica set nginx-deployment-67cd6d5bcb to 6
  Normal  ScalingReplicaSet  19s (x2 over 17m)   deployment-controller  Scaled up replica set nginx-deployment-5f4847564d to 3
  Normal  ScalingReplicaSet  19s                 deployment-controller  Scaled down replica set nginx-deployment-67cd6d5bcb to 3
  Normal  ScalingReplicaSet  17s                 deployment-controller  Scaled down replica set nginx-deployment-65b5678957 to 1
  Normal  ScalingReplicaSet  17s                 deployment-controller  Scaled up replica set nginx-deployment-5f4847564d to 4
  Normal  ScalingReplicaSet  17s                 deployment-controller  Scaled down replica set nginx-deployment-65b5678957 to 0
  Normal  ScalingReplicaSet  17s                 deployment-controller  Scaled up replica set nginx-deployment-5f4847564d to 5

```
