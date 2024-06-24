## Probes

- Liveness Probe (With some delay permanently checks if app works. If false reboot)
  - Controls app state while it works.
  - Executing permanently
- Readliness Probe (Checks if some endpoint ready to accept traffic. If false pod'll be out of traffic)
  - Checks if ready to accept traffic
  - If false executing removes from balancing
  - Executing permanently
- Startup Probe(If app take some time to init wait to start. Liveness and Readliness will execute only after Startup success)
  - Checks if app started
  - Executed on start

```yaml
readinessProbe:
  failureThreshold: 3 # number of errors which k8 allows before remove pod from balancing
  httpGet: # type of check(httpGet, exec, tcpSocket). Success code 200-399
    path: /
    port: 80
  periodSeconds: 10 # check period
  successThreshold: 1 # quantity of success to drop counter failtTresh
  timeoutSeconds: 1 # exec check timeout
livenessProbe:
  failureThreshold: 3
  httpGet:
    path: /
    port: 80
  periodSeconds: 10
  successThreshold: 1
  timeoutSeconds: 1
  initialDelaySeconds: 10 # was usefull before startupProbe was presented
startupProbe:
  httpGet:
    path: /
    port: 80
  failureThreshold: 30 # allows 30 fails with period 10 sec
  periodSeconds: 10
```

# Publication

- Service: L3 OSI, NAT, kube-proxy
- Ingress: L7 OSI, HTTP/HTTPS, nginx, envoy, haproxy
