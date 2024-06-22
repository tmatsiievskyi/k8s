```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-app
  strategy:
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
    type: RollingUpdate
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - image: quay.io/testing-farm/nginx:1.12
          name: nginx
          env:
            - name: TEST # name
              value: foo # value
          ports:
            - containerPort: 80
          resources:
            requests:
              cpu: 10m
              memory: 100Mi
            limits:
              cpu: 100m
              memory: 100Mi
```

## ConfigMap

- ConfigMap. key:value, yaml file
- ConfigMap.data - stores key:values

```yaml
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-configmap-env
data:
  dbhost: postgresql
  DEBUG: 'false'
```

```shell
kubectl apply -f configmap.yaml
```

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-app
  strategy:
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
    type: RollingUpdate
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - image: quay.io/testing-farm/nginx:1.12
          name: nginx
          env:
            - name: TEST
              value: foo
          envFrom: #outside
            - configMapRef: #link to cm
                name: my-configmap-env
          ports:
            - containerPort: 80
          resources:
            requests:
              cpu: 10m
              memory: 100Mi
            limits:
              cpu: 100m
              memory: 100Mi
```

# get container env

```yaml
Environment Variables from:
  my-configmap-env  ConfigMap  Optional: false
```

```shell
kubectl get cm my-configmap-env -o yaml
apiVersion: v1
data:
  DEBUG: "false"
  dbhost: postgresql
kind: ConfigMap
```

```shell
kubectl exec -it my-deployment-7d7cff784b-rjb55 -- bash
```
