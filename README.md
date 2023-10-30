# quepid-kubernetes

Basic Kubernetes setup for https://quepid.com/

## Install

### k8s Namespace

```
kubectl create namespace quepid
```

### Edit configmap

Edit `k8s/00-credentials.yml` and enter proper credentials.


### Install k8s manifests

```
kubectl apply -f k8s/00-configmap.yml
kubectl apply -f k8s/00-credentials.yml
kubectl apply -f k8s/01-deployment.yml
kubectl apply -f k8s/02-service.yml
```

## Database

go inside pod

```
➜  kubectl get pods -n quepid
NAME                      READY   STATUS    RESTARTS   AGE
quepid-5569f5fbcc-gbc9w   1/1     Running   0          6d19h

➜  kubectl exec -n quepid --stdin --tty quepid-5569f5fbcc-gbc9w -- /bin/bash
root@quepid-5569f5fbcc-gbc9w:/srv/app#

```

### Empty DB

Inside running pod

```
rake db:setup
rake db:migrate
```

### Existing DB

Inside running pod

```
rake db:schema:load
rake db:migrate
```

## TODO

- make helm chart
- add ingress / ssl offloading

## SSL certificate

https://cert-manager.io/docs/installation/kubectl/