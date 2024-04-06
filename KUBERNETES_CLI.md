# Setting up Kubernetes client

Assumptions:

- you have a Kubernetes cluster running in cloud
- you have proper account and credentials in cloud

## Cli tool

```
brew install kubernetes-cli
```

## AWS

```
pip3 install awscli --upgrade
aws configure  (you have to specify credentials - api key and secret - created in AWS IAM)
aws eks --region eu-west-1 update-kubeconfig --name <kubernetes cluster namer>
```

## GCP

[Install kubectl and configure cluster access ](https://cloud.google.com/kubernetes-engine/docs/how-to/cluster-access-for-kubectl)

## Testing access

```
kubectl get namespaces
```
