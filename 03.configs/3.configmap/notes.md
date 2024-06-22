## ConfigMap

- allows key: value(which will be transpiled to the file)

```shell
k apply -f deployment-with-configmap.yaml
k exec -it my-deployment-5bc5bc7b46-gvbqd -- bash
cd /etc/nginx
cd conf.d
ls -> default.conf
```

k port-forward my-deve- 8080:80 & - creates proxy which sends request from localhost into k8s cluster
[8080] - localport which k will listen
[80] - pod port where we will send data
