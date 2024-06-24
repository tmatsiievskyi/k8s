## Services

- Static IP
- DNS name in kube-dns on that ip
- Service - iptables for routing

- ClusterIP
- NodePort
- LoadBalancer
- ExternalName
- ExternalIPS

- Service selector should match pod label where we need to send traffic
- Pod and Service should be in the same namespace

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service # will be stored in DNS, and should be used
spec:
  selector:
    app: my-app # pods labels where we should send traffic
  type: ClusterIP
  ports:
    - port: 80 # listen
      targetPort: 80 # sends traffic
      protocol: TCP
```

# ClusterIP (type: ClusterIP)

Pod Communication in cluster

```shell
k port-forward service/my-service 3000:80
```

# NodePort (type: NodePort)

Public app outside
type: NodePort

attach for each node specific port. All traffic will go through that port(30000 - 32676)

Use for Balancer(Nginx) and LoadBalancer

# LoadBalance (type: ...)

Usually cloud providers

Controller(openstack) check k8s for LB. WHen LB creates Balancer which receive traffic which sends to app

# ExternalName

reverse communication

# ExternalIPS

Same as NodePort but with ip

# Headless

ClusterIP: none

# Ingress Controller

- Nginx which works in cluster. Receives Req and sends to app
