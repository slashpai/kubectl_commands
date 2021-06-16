# Pod commands

```bash
alias k=kubectl
```

## Create pod

```go
controlplane $ k run nginx --image nginx
pod/nginx created
controlplane $ k get po
NAME    READY   STATUS              RESTARTS   AGE
nginx   0/1     ContainerCreating   0          4s
controlplane $ k get po -w
NAME    READY   STATUS    RESTARTS   AGE
nginx   1/1     Running   0          15s
```

## Get image used for pod

```go
controlplane $ k get po -o custom-columns="name:metadata.name,image:spec.containers[*].image"
name            image
newpods-99chb   busybox
newpods-mrmpz   busybox
newpods-vp9rk   busybox
nginx           nginx
```

## Get nodes on which pods running

```go
controlplane $ k get po -o custom-columns="name:metadata.name,image:spec.containers[*].image,node:spec.nodeName"
name            image     node
newpods-99chb   busybox   node01
newpods-mrmpz   busybox   node01
newpods-vp9rk   busybox   node01
nginx           nginx     node01
```

or

```go
controlplane $ k get po -o wide
NAME            READY   STATUS    RESTARTS   AGE     IP           NODE     NOMINATED NODE   READINESS GATES
newpods-99chb   1/1     Running   0          9m51s   10.244.1.3   node01   <none>           <none>
newpods-mrmpz   1/1     Running   0          9m51s   10.244.1.5   node01   <none>           <none>
newpods-vp9rk   1/1     Running   0          9m50s   10.244.1.4   node01   <none>           <none>
nginx           1/1     Running   0          10m     10.244.1.2   node01   <none>           <none>
controlplane $ 
```

## Get reason for pod error

```go
controlplane $ k get po -o custom-columns="name:metadata.name,stats:status.containerStatuses[*].state"
name            stats
newpods-99chb   map[running:map[startedAt:2021-06-16T05:09:00Z]]
newpods-mrmpz   map[running:map[startedAt:2021-06-16T05:09:02Z]]
newpods-vp9rk   map[running:map[startedAt:2021-06-16T05:09:01Z]]
nginx           map[running:map[startedAt:2021-06-16T05:08:40Z]]
webapp          map[waiting:map[message:Back-off pulling image "agentx" reason:ImagePullBackOff]],map[running:map[startedAt:2021-06-16T05:20:16Z]]
```
