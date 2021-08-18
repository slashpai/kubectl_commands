# RBAC

```bash
alias k=kubectl
```

## Create namespace

```go
k create ns red
k create ns blue
```

## Create Role in Red Namespace

```go
k -n red create role secret-manager --verb get,list --resource secret
```

## Create RoleBinding in Red Namespace

```go
 k -n red create rolebinding secret-manager --role secret-manager --user alice
```

## Test in Red Namespace

```go
k -n red auth can-i get secrets --as alice
k -n red auth can-i list secrets --as alice
```

## Create Role in Blue Namespace

```go
k -n blue create role secret-manager --verb watch --resource secret
```

## Create RoleBinding in Blue Namespace

```go
k -n blue create rolebinding secret-manager --role secret-manager --user alice
```

## Test in Blue Namespace

```go
k -n blue auth can-i watch secrets --as alice
```

## Create ClusterRole

```go
k create clusterrole deploy-deleter --verb=delete --resource=deployment
```

## Create ClusterRoleBinding

```go
k create clusterrolebinding deploy-deleter --clusterrole=deploy-deleter --user=alice
```

## Create RoleBinding to give access only to single namespace

```go
 k -n red create rolebinding deploy-deleter --clusterrole=deploy-deleter --user=jane
 ```
 
## Test ClusterRole and RoleBindings

```go

░▒▓    ~  k auth can-i delete deployment --as alice                                     ✔  2.7.17   1.16.4   2.7.3   at minikube ⎈  at 13:43:01  ▓▒░
yes

░▒▓    ~  k auth can-i delete deployment --as alice -A                                  ✔  2.7.17   1.16.4   2.7.3   at minikube ⎈  at 13:43:14  ▓▒░
yes

░▒▓    ~  k auth can-i delete deployment --as alice -n red                              ✔  2.7.17   1.16.4   2.7.3   at minikube ⎈  at 13:43:20  ▓▒░
yes

░▒▓    ~  k auth can-i delete deployment --as jane                                      ✔  2.7.17   1.16.4   2.7.3   at minikube ⎈  at 13:43:24  ▓▒░
no

░▒▓    ~  k auth can-i delete deployment --as jane -n red                             1 ✘  2.7.17   1.16.4   2.7.3   at minikube ⎈  at 13:43:34  ▓▒░
yes

░▒▓    ~ 
```
