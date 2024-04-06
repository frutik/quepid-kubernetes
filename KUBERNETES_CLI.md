# Setting up Kubernetes client

Assumptions:

- you have a Kubernetes cluster running in cloud
- you have proper account and credentials in cloud

## AWS

### Setting up access

```
brew install kubernetes-cli
pip3 install awscli --upgrade
aws configure  (you have to specify credentials - api key and secret - created in AWS IAM)
aws eks --region eu-west-1 update-kubeconfig --name <kubernetes cluster namer>
```

### Test access

```
kubectl get namespaces
```
