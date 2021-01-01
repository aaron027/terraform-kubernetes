# terraform-kubernetes (AWS)


This repo has some resources to start using Kubernetes and terraform.
When you finish this tutorial, you will have this resources created on you AWS account:

* EKS cluster
* VPC, Subnets, Nat gateway, etc
* IAM policy
* 2 kubernetes pods for tests
* 1 AWS Load balancer
* 1 ingress-nginx to receive requests and proxy for pods


First, we need to clone the repo and init terraform:
```
git clone git@github.com:renandeandradevaz/terraform-kubernetes
cd terraform-kubernetes
terraform init
```

Then, we have to apply the resources on the cloud:
```
terraform apply
```

Create an environment variable to keep kubeconfig data:
```
export KUBECONFIG=${PWD}/kubeconfig_my-cluster
```

Create the nginx ingress inside the cluster:
```
kubectl apply -f ingress-nginx.yaml
```

Deploy two containers for testing:
```
kubectl apply -f banana.yaml
```
```
kubectl apply -f apple.yaml
```

Deploy the routes:
```
kubectl apply -f routes.yaml
```

If you want to activate the auto scaler. You must run:
```
kubectl apply -f auto-scaler.yaml
```


After create all the resources, now it's time to test.
Go to https://console.aws.amazon.com/ec2/v2/home#LoadBalancers and copy the DNS name


Test if application returns something:
```
curl {AWS_LOAD_BALANCER_URL}/banana
``` 
and 
```
curl {AWS_LOAD_BALANCER_URL}/apple
``` 

In the end, you can remove all created resources to save money:

```
terraform destroy
```

