# quepid-kubernetes

Basic Kubernetes setup for https://quepid.com/

## Kubectl

Setting up [Kubectl](KUBERNETES_CLI.md)

## External dependencies

This setup assumes that external dependencies (Redis and MySQL) are provided as a service by AWS/GCP or set up in the Kubernetes cloud separately.

[How to create Redis instance in AWS ElastiCache](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Clusters.Create.html#Clusters.Create.CON.RedisCluster)
[How to create Mysql in AWS RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_CreateDBInstance.html)

## Install

### k8s Namespace

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

The only recommended way to use atm

```
helm template frutik-quepid charts/quepid | kubectl apply -n quepid -f -
```

Replace `frutik-quepid` by some identifier for this instance of quepid.
For example 'x-y-z-quepid-dev'.

You can use the --set flag to override the configuration in values.yaml. For instance, 
if you would like to set a custom secret for the Rails app

```
helm template frutik-quepid charts/quepid --set credentials.secret_key_base=qweasd | kubectl apply -n quepid -f - 
```

or from a local file with values specific for 
your environment (structure should match)

```
helm template frutik-quepid charts/quepid --values my_values.yml | kubectl apply -n quepid -f -
```

See the example of `my_values.yml` committed in this repo.

## SSL certificate

https://cert-manager.io/docs/installation/kubectl/
