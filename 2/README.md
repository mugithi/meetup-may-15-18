### Install Load Balancer

```
kubectl apply -f https://raw.githubusercontent.com/google/metallb/v0.6.2/manifests/metallb.yaml
```

#### Install a range of IP addresses that will be used as load balancing end points

```
kubectl apply -f - EOF
---
apiVersion: v1
kind: ConfigMap
metadata:
  namespace: metallb-system
  name: config
data:
  config: |
    address-pools:
    - name: my-ip-space
      protocol: layer2
      addresses:
      - 10.20.44.200-10.20.44.250

EOF
```
