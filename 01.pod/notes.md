- POD - k8s abstraction. One running service in k8s. 1 instance. Could have few containers.
  includes 2 containers under the hood
  &emsp app container
  - POD_name_hash - network namespaces
  - networks for our app container
- containers in one pod can communicate by localhost

  - container in pod exists in scope of one node
  - few containers in pod is antipattern
    but cann be used for: - requirements
    - scaling linear - strong connection
    - prometheus, config reloader - monitoring tools - auth services(proxy)

```yaml
---
apiVersion: v1 # api verion
kind: Pod # type described obj
metadata: # addditional info about obj
  name: my-pod
spec: # obj details inside
  containers:
    - image: quay.io/testing-farm/nginx:1.12
      name: nginx
      ports:
        - containerPort: 80
```

create pod from file:

```shell
kubectl create -f pod.yaml
```

command:

```shell
kubectl get pod
```

result:

```shell output
NAME     READY   STATUS    RESTARTS   AGE
my-pod   1/1     Running   0          108s
```

- READY - number of replicas
- RESTARTS - kubernetes can restart pods. Counter of restarts

- in one namespace pod name should be unique

pod info

```shell
kubectl describe pod my-pod
```

pod delete

```shell
kubectl delete pod my-pod
```

pod delete by file

```shell
kubectl delete -f pod.yaml
```
