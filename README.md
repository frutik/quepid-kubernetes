# quepid-kubernetes

Basic Kubernetes setup for https://quepid.com/

## Kubectl

Setting up [Kubectl](KUBERNETES_CLI.md)

## External dependencies

This setup assumes that external dependencies (Redis and MySQL) are provided as a service by AWS/GCP or set up in the Kubernetes cloud separately.

[How to create Redis instance in AWS ElastiCache](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Clusters.Create.html#Clusters.Create.CON.RedisCluster)
[How to create Mysql in AWS RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_CreateDBInstance.html)

## Install

### Create k8s Namespace

```
kubectl create namespace quepid
```

### Edit configmap

Edit `k8s/00-credentials.yml` and enter proper credentials to access external dependencies.

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

You should understand all the risks of loading db schema into an existing not empty database. So, inside the running pod

```
rake db:schema:load
rake db:migrate
```

## How to use

### Locally

```
kubectl port-forward service/quepid 8080:80 -n quepid
```

Now you can open http://127.0.0.1:8080/ in your browser.

### Expose to the outside world

Edit `k8s/03-ingress.yml` and enter proper values.

Create ingress

```
kubectl apply -f k8s/03-ingress.yml
```

## Helm

NOT READY YET

### Update credentials for external dependencies

```
nano my_values.yml
```

### create kubernetes namespace

As described previously

### The only recommended way to use atm

```
helm template frutik-quepid charts/quepid --values my_values.yml | kubectl apply -n quepid -f -
```

Replace `frutik-quepid` with some identifier for this _instance_ of quepid.
For example 'x-y-z-quepid-dev'.


### SSL certificate

https://cert-manager.io/docs/installation/kubectl/
