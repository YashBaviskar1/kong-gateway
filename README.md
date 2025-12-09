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

1) Kong provides a Helm chart for deploying Kong Gateway. Add the `charts.konghq.com repository` and run `helm repo update` to ensure that you have the latest version of the chart.
    ```bash
    helm repo add kong https://charts.konghq.com
    helm repo update
    ```


asssumption : everything kong related to be store in kong namespace 
```bash 
kubectl create namespace kong
```

2) It is Reccomanded to have external db service attached to it, though one can use Cloud Native Postgres Operator, though right now there is default implmentation uses external postgres db. 

file : `new-postgres.yaml`
```bash 
kubectl apply -f new-postgres.yaml 
```

this will create a postgres pod in the kong namespace.


3) then apply th `values.yaml` with the the details of the postgres added in the `env` variable in the yaml 

file : `values.yaml`
```bash
helm install kong kong/kong -f values.yaml -n kong
```


4) After applying we can see in the pods that the containers are initalisated 


5) Reccomand bootstrapping : Since we are using an external db config, sometimes it needs to be manually bootstraped to the kong 

```bash 
kubectl exec -it <pod-name-of-kong> -n kong -- kong migrations bootstrap
```


### Verification 

In order to verify whether the kong gateway is active or not we can do : 
```bash 
kubectl get svc -n kong
```

This will get all the running servies, the main one to target here is something along the lines of `kong-admin` service. 

```bash 
kubectl port-forward svc/kong-admin -n kong 8001:8443
```

This will port-forward the admin proxy on `localhost:8001`

which could be verified by simple curl request :
```bash 
curl -k https://localhost:8001/status
```

which will get us the information about the database config in JSON, it means everything is running fine. 