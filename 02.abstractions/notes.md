Replicaset and Deployment

## ReplicaSet

- Obj, which presents replicas of our app
- Inside ReplicaSet yaml template of pods, which will be creating
- When pods will be creating, special label will be attached to them. According to that label ReplicaSet will clarify if this is it own pods.
- ReplicaSet has info about selector. It will compare pod to selector.
- Have field which have value about how much pods we want to create

```yaml
---
# file: practice/2.application-abstractions/2.replicaset/replicaset.yaml
apiVersion: apps/v1 # Which version of the Kubernetes API you're using to create this object
kind: ReplicaSet # What kind of object you want to create
metadata:
  name: my-replicaset
spec:
  replicas: 2 # how many replicas
  selector: # allows to select pods with a matched label ('my-app' here)
    matchLabels:
      app: my-app
  template: # template of pod we want to create, you don't need a name, k8s will generate it
    metadata:
      labels: # you can use labels anywhere
        app: my-app
    spec: # to describe k8s what you want to build
      containers:
        - image: quay.io/testing-farm/nginx:1.12
          name: nginx # name is have to be unique
          ports:
            - containerPort: 80
```

```shell
kubectl delete pod --all # delete all pods
kubectl delete replicaset --all # delete all repliceSet
kubectl delete all --all -A # all namespaces delete obj, equal rm -rf
```

# Difference create и apply

- create - executes only 1 time. Next error
- apply
  - apply changes
  - for -f not only file but also directory
  - usually using in pipelines CI/CD

create replicaset

```shell
kubectl apply -f replicaset.yaml
```

replicaset info

```shell
kubectl get replicasets
```

result:

```shell
NAME            DESIRED   CURRENT   READY   AGE
my-replicaset   2         2         2       2m25s
```

- DESIRED - desired replicas
- CURRENT - current replicas
- READY - 2 pods are ready and awailable

scale

```shell
kubectl scale --replicas 3 replicaset my-replicaset
```

- replicaSet checks pods by labels in namespace. If pod with same label has been created seperatly replicaSet will terminate it.

# Update image in replicaSet

- update yaml and [apply]
- kubectl set image replicaset my-replicaset nginx=quay.io/testing-farm/nginx:1.13
- kubectl set image replicase my-replicaset '\*=quay.io/testing-farm/nginx:1.13'

existing pods will have the old version. Changes'll be apllied only for new pods.

describe:

```shell
kubectl describe replicaset my-replicaset
```

explain:

```shell
kubectl explain pod
kubectl explain pod.spec
```

## Deployment

- Abstraction over ReplicaSets
- Should be used for app update
- spec same as ReplicaSet but different behaviour
- Deployment creates for us ReplicaSet, generates name and creates 2 pods in ReplicaSet, adding hash part
- Updating Deployament will also create new Replicaset
- Old ReplicaSet will not be deleted to have possibbility to rollout

# update image

- update yaml
- kubectl set image deployment my-deployment '\*=...'
- kubectl edit deployment my-deployment

Rollout:

```shell
kubectl rollout undo deployment my-deployment
```

# Rollout Strategy

- RevisionHistoryLimit - default 10.
- kubectl explain deployment.spec - contains [strategy]
- kubectl explain deployment.spec.strategy - contains [rollingUpdate], [type.Recreate], [type.RollingUpdate]
  - [RollingUpdate] - replicas creates step by step withoud downtime. App should support working with 2 versions
  - [maxSurge] - number or percent of pods which could be higher than documented number.
    Example: replicas: 2 и maxSurge: 1. Update: 3
  - [maxUnavailable] - number of unavailable pods.
    Example: replicas: 2 и maxUnavailable: 1. One can be deleted.
    Example: maxSurge: 1, maxUnavailable: 1, replicas: 2.
    Result: When 1 old replic deletes. 2 new creates. K8s waits that 2, than delete 1.
  - [Reacreate] - delete everything old then create new. Creates downtime

## Namespace

- Split cluster to namespaces.

## Resources

- Limits
- quantity. Quantity of resources which POD can use. Top Margin
- requests. Quantity of resources reserved for POD on a Node. Not splitting between PODs on Node

- cpu - m(milCPU) - 1m(1/1000 CPU)
- memory - Mi(megabytes)

```shell
kubectl desctibe pod [pod]
```

contains:
Qos Class:

- Burstable (Requests but without limits. Limits higher than requests). Will be deleted in second order
- Best Effort (no limits). No memory, will be deleted first
- Garantied (request === limits). Last to delete

# Edit rources

- Update deployment file. Apply
- Kubectl edit deployment. Save
- ```shell
  kubectl patch deployment my-deployment --patch '{"spec":{"template":{"spec":{"containers":[{"name":"nginx","resources":{"requests":{"cpu":"100"},"limits":{"cpu":"100"}}}]}}}}'
  ```
