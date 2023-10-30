# quepid-kubernetes

Basic Kubernetes setup for https://quepid.com/

## Database

go inside pod

```
➜  quepid-kubernetes git:(main) kubectl get pods -n quepid
NAME                      READY   STATUS    RESTARTS   AGE
quepid-5569f5fbcc-gbc9w   1/1     Running   0          6d19h

➜  quepid-kubernetes git:(main) ✗ kubectl exec -n quepid --stdin --tty quepid-5569f5fbcc-gbc9w -- /bin/bash
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