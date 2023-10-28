# quepid-kubernetes

Basic Kubernetes setup for https://quepid.com/

## Database

go inside pod

```
kubectl get pod
```

### Empty

```
rake db:setup
rake db:migrate
```

### Existing

```
rake db:schema:<TODO>
rake db:migrate
```

## TODO

- finish db setup instrictions
- make helm chart
- add ingress / ssl offloading
