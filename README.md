# Kong Setup + Gateway Auth 

## Introduction
Kong is used as an Cloud Native api-gateway for enabling reverse proxy and managining multiple services in your infra

## Setup 

the prerequisites for kong is that it requires, to setup the Kong Gateway, for now, the KIC (Kong Ingress Controller) is skipped and rather to access the underlying sevices, we shall utilise simple port forwarding.

Note that this approach may not be production friendly 

### Prequisties : 
- K8 Cluster (used minikube)
- helm 

### Steps 

Kong provides a Helm chart for deploying Kong Gateway. Add the `charts.konghq.com repository` and run `helm repo update` to ensure that you have the latest version of the chart.
```bash
helm repo add kong https://charts.konghq.com
helm repo update
```


