# EKS BY MANUALLY

## 1. Creating a VPC for your EKS cluster by cloudformation:
- https://s3.us-west-2.amazonaws.com/amazon-eks/cloudformation/2020-10-29/amazon-eks-vpc-private-subnets.yaml

## 2. Creating a EKS cluster
Note: First create a role for EKS Cluster:
- AmazonEKSClusterPolicy
second create a role for Worker node group:
- AmazonEKS_CNI_Policy
- AmazonEKSWorkerNodePolicy
- AmazonEC2ContainerRegistryReadOnly

Please refer https://docs.aws.amazon.com/eks/latest/userguide/create-cluster.html   to create EKS cluster

![image](https://user-images.githubusercontent.com/50055329/180602131-6b8679cb-1652-4af1-8e49-b1fca5481726.png)
