# EKS BY MANUALLY/MANAGEMENT CONSOLE

## 1. Creating a VPC for your EKS cluster by cloudformation:
- https://s3.us-west-2.amazonaws.com/amazon-eks/cloudformation/2020-10-29/amazon-eks-vpc-private-subnets.yaml

## 2. IAM role for EKS cluster and worker node
First create a role for EKS Cluster:
- AmazonEKSClusterPolicy

Second create a role for Worker node group:
- AmazonEKS_CNI_Policy
- AmazonEKSWorkerNodePolicy
- AmazonEC2ContainerRegistryReadOnly
- 
## 3. Creating a EKS cluster

Please refer https://docs.aws.amazon.com/eks/latest/userguide/create-cluster.html   to create EKS cluster

## 4. Creating a Worker node

![image](https://user-images.githubusercontent.com/50055329/180602131-6b8679cb-1652-4af1-8e49-b1fca5481726.png)

===========================================================================================
# Now I am going to create a workstation

## Install the AWS CLI on Linux

- curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
- unzip awscliv2.zip
- sudo ./aws/install
- ./aws/install -i /usr/local/aws-cli -b /usr/local/bin
- aws --version

## aws configure

Note: (Access and Secrete key should be same the user which you had created eks cluster) 

##  Install kubectl

- curl -o kubectl https://s3.us-west-2.amazonaws.com/amazon-eks/1.22.4/2021-07-05/bin/linux/amd64/kubectl
- chmod +x ./kubectl
- mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$PATH:$HOME/bin
- echo 'export PATH=$PATH:$HOME/bin' >> ~/.bashrc
- kubectl
- kubectl version --short --client

## Installing aws-iam-authenticator for eks
- curl -o aws-iam-authenticator https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/aws-iam-authenticator
- chmod +x ./aws-iam-authenticator &&  mkdir -p $HOME/bin && cp ./aws-iam-authenticator $HOME/bin/aws-iam-authenticator && export PATH=$PATH:$HOME/bin && echo 'export PATH=$PATH:$HOME/bin' >> ~/.bashrc
- aws-iam-authenticator help
- kubectl cluster-info

## to connect with EKS cluster we have two ways
## First way:
- aws eks --region us-east-1 describe-cluster --name Dmat-onp-cluster --query cluster.status
- aws eks --region us-east-1 update-kubeconfig --name Dmat-onp-cluster
- kubectl get svc

## Second way:
- aws configure list
- aws eks update-kubeconfig --name Dmat-onp-cluster

===========================================================================================
# Now I am going to create a EKS cluster autoscaler
## How to install Cluster Autoscaler on AWS EKS
Creating IAM role with this name "nodegroup-autoscale-policy" and will be attached with previous worker node's role
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "sts:AssumeRole",
                "autoscaling:DescribeAutoScalingGroups",
                "autoscaling:DescribeAutoScalingInstances",
                "autoscaling:DescribeLaunchConfigurations",
                "autoscaling:DescribeTags",
                "autoscaling:SetDesiredCapacity",
                "autoscaling:TerminateInstanceInAutoScalingGroup",
                "ec2:DescribeLaunchTemplateVersions"
            ],
            "Resource": "*",
            "Effect": "Allow"
        }
    ]
}
```
## Downloading manifest file
- curl -o cluster-autoscaler-autodiscover.yaml https://raw.githubusercontent.com/kubernetes/autoscaler/master/cluster-autoscaler/cloudprovider/aws/examples/cluster-autoscaler-autodiscover.yaml

- kubectl apply -f cluster-autoscaler-autodiscover.yaml

- kubectl get deployment -n kube-system cluster-autoscaler 

- kubectl edit deployment -n kube-system cluster-autoscaler

![image](https://user-images.githubusercontent.com/50055329/180605050-2ebb6a10-1ea4-4447-b709-5ddc8a30c553.png)

![image](https://user-images.githubusercontent.com/50055329/180605132-c68120bd-2abb-43b9-9fe7-32b685a3938b.png)

![image](https://user-images.githubusercontent.com/50055329/180605288-2657eb0a-ceab-4f9d-9cb3-3b407606112d.png)

===========================================================================================
# Now I going to deploy nginx application
## k8s cluster is up and running

- kubectl create deployment nginx --image=nginx --replicas=2

- kubectl expose deployment nginx --port=80 --target-port=80 --name=service --type=LoadBalancer

- kubectl get pod

- kubectlget svc

![image](https://user-images.githubusercontent.com/50055329/180606488-94828dfa-69c9-401d-9ad8-cca4fb4b14d2.png)

- kubectl exit deployment nginx

![image](https://user-images.githubusercontent.com/50055329/180606972-8d1c36f4-fd96-444d-8e3d-57bedf2cdd5f.png)

![image](https://user-images.githubusercontent.com/50055329/180606995-e80839a3-adbe-4988-81c2-cbbf06250436.png)
