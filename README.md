# DashMachine K8S Base Manifest


These are the base manifests to deploy [DashMachine](https://github.com/rmountjoy92/DashMachine) to k8s. 

It uses a persistent volume claim for the config / uploads so they persist deploy to deployment.

## Notes

Expected to used with Kustomizer. You can setup a local file and add ingress or other items needed.

## Example

Setting up a local deployment that also has a

kustomization.yaml
```
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization


namespace: dashmachine

commonLabels:
  app: dashmachine

resources:
  - github.com/beckje01/dashmachine-k8s/manifests?ref=v0.1
  - ingress.yaml
```

ingress.yaml
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: dashmachine-ui
  annotations:
    kubernetes.io/ingress.class: traefik
spec:
  rules:
  - host: dash.lab.example
    http:
      paths:
      - backend:
          serviceName: dashmachine
          servicePort: 5000
```

Build:
```
kustomize build ./ | kubectl apply -f -
```