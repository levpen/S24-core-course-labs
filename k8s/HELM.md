## Task 1
```
$ helm install my-app-helm-python my-app/ --values my-app/values.yaml
```

```bash
NAME: my-app-helm-python
LAST DEPLOYED: Tue Apr  9 22:37:31 2024
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:
1. Get the application URL by running these commands:
     NOTE: It may take a few minutes for the LoadBalancer IP to be available.
           You can watch the status of by running 'kubectl get --namespace default svc -w my-app-helm-python'
  export SERVICE_IP=$(kubectl get svc --namespace default my-app-helm-python --template "{{ range (index .status.loadBalancer.ingress 0) }}{{.}}{{ end }}")
  echo http://$SERVICE_IP:5000
```

```
$ kubectl get pods,svc
```

```bash
NAME                                      READY   STATUS    RESTARTS   AGE
pod/my-app-helm-python-55447f457b-nr7qg   1/1     Running   0          4m25s

NAME                         TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)          AGE
service/kubernetes           ClusterIP      10.96.0.1     <none>        443/TCP          8d
service/my-app-helm-python   LoadBalancer   10.101.2.11   <pending>     5000:31431/TCP   4m25s

```

## Task 2

```
kubectl get po
```
```bash
NAME                                             READY   STATUS      RESTARTS   AGE
my-app-with-hooks-helm-python-86b6b6b64d-f44kp   1/1     Running     0          24s
postinstall-hook                                 0/1     Completed   0          24s
preinstall-hook                                  0/1     Completed   0          46s
```

```
kubectl describe po preinstall-hook
```
```bash
Name:             preinstall-hook
Namespace:        default
Priority:         0
Service Account:  default
Node:             minikube/192.168.49.2
Start Time:       Wed, 10 Apr 2024 00:16:31 +0300
Labels:           <none>
Annotations:      helm.sh/hook: pre-install
Status:           Succeeded
IP:               10.244.0.32
IPs:
  IP:  10.244.0.32
Containers:
  pre-install-container:
    Container ID:  docker://5218e4bb8933cccc2e81d4d3eff5dfecf3f35544fc340e3c27905317ba38a7bd
    Image:         busybox
    Image ID:      docker-pullable://busybox@sha256:c3839dd800b9eb7603340509769c43e146a74c63dca3045a8e7dc8ee07e53966
    Port:          <none>
    Host Port:     <none>
    Command:
      sh
      -c
      echo The pre-install hook is running && sleep 20
    State:          Terminated
      Reason:       Completed
      Exit Code:    0
      Started:      Wed, 10 Apr 2024 00:16:31 +0300
      Finished:     Wed, 10 Apr 2024 00:16:51 +0300
    Ready:          False
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-tj99t (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             False 
  ContainersReady   False 
  PodScheduled      True 
Volumes:
  kube-api-access-tj99t:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  2m    default-scheduler  Successfully assigned default/preinstall-hook to minikube
  Normal  Pulled     2m    kubelet            Container image "busybox" already present on machine
  Normal  Created    2m    kubelet            Created container pre-install-container
  Normal  Started    2m    kubelet            Started container pre-install-container
```

```
kubectl describe po postinstall-hook
```
```bash
Name:             postinstall-hook
Namespace:        default
Priority:         0
Service Account:  default
Node:             minikube/192.168.49.2
Start Time:       Wed, 10 Apr 2024 00:16:53 +0300
Labels:           <none>
Annotations:      helm.sh/hook: post-install
Status:           Succeeded
IP:               10.244.0.34
IPs:
  IP:  10.244.0.34
Containers:
  post-install-container:
    Container ID:  docker://95fa221cbb70227e9d30f53356199b685efacf92b0970987f0f349f760ec86fb
    Image:         busybox
    Image ID:      docker-pullable://busybox@sha256:c3839dd800b9eb7603340509769c43e146a74c63dca3045a8e7dc8ee07e53966
    Port:          <none>
    Host Port:     <none>
    Command:
      sh
      -c
      echo The post-install hook is running && sleep 15
    State:          Terminated
      Reason:       Completed
      Exit Code:    0
      Started:      Wed, 10 Apr 2024 00:16:58 +0300
      Finished:     Wed, 10 Apr 2024 00:17:13 +0300
    Ready:          False
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-tr5m2 (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             False 
  ContainersReady   False 
  PodScheduled      True 
Volumes:
  kube-api-access-tr5m2:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age    From               Message
  ----    ------     ----   ----               -------
  Normal  Scheduled  2m50s  default-scheduler  Successfully assigned default/postinstall-hook to minikube
  Normal  Pulling    2m49s  kubelet            Pulling image "busybox"
  Normal  Pulled     2m45s  kubelet            Successfully pulled image "busybox" in 4s (4s including waiting)
  Normal  Created    2m45s  kubelet            Created container post-install-container
  Normal  Started    2m45s  kubelet            Started container post-install-container
```

```
kubectl get pods,svc
```
```bash
NAME                                                 READY   STATUS      RESTARTS   AGE
pod/my-app-with-hooks-helm-python-86b6b6b64d-f44kp   1/1     Running     0          6m52s
pod/postinstall-hook                                 0/1     Completed   0          6m52s
pod/preinstall-hook                                  0/1     Completed   0          7m14s

NAME                                    TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
service/kubernetes                      ClusterIP      10.96.0.1       <none>        443/TCP          8d
service/my-app-with-hooks-helm-python   LoadBalancer   10.105.29.249   <pending>     5000:32466/TCP   6m52s
```

Adding hook-delete-policy:
```bash
annotations:
    "helm.sh/hook-delete-policy": hook-succeeded
```
