## Secret

- structure: key:value
- types:
  - generic (logins, passwords, ...etc)
  - docker-registry (auth data in docker registry)
  - tls (tls certificates)
  - data stored in base64

# Creating secret from terminal

```bash
kubectl create secret generic test --from-literal=test1=asdf --from-literal=dbpassword=1q2w3e
kubectl get secret
kubectl get secret test -o yaml
```

get env:

```bash
kubectl exec -it [depl] -- base
env
```
