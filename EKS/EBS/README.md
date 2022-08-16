# EKS Storage with EBS - Elastic Block Store

## Step-01: Introduction
- Create IAM Policy for EBS
- Associate IAM Policy to Worker Node IAM Role
- Install EBS CSI Driver

## Step-02:  Create IAM policyy

- vim example-iam-policy.json

```json

{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:AttachVolume",
        "ec2:CreateSnapshot",
        "ec2:CreateTags",
        "ec2:CreateVolume",
        "ec2:DeleteSnapshot",
        "ec2:DeleteTags",
        "ec2:DeleteVolume",
        "ec2:DescribeInstances",
        "ec2:DescribeSnapshots",
        "ec2:DescribeTags",
        "ec2:DescribeVolumes",
        "ec2:DetachVolume"
      ],
      "Resource": "*"
    }
  ]
}
```

## Step-03: Create the policy. You can change AmazonEKS_EBS_CSI_Driver_Policy to a different name, but if you do, make sure to change it in later steps too.
```
aws iam create-policy \
    --policy-name AmazonEKS_EBS_CSI_Driver_Policy \
    --policy-document file://example-iam-policy.json
```
## Step-04: Create an IAM role and attach the IAM policy to it. You can use either eksctl or the AWS CLI:

## Step-05: Create iamserviceaccount:
Replace my-cluster with the name of your cluster and 111122223333 with your account ID.
```
eksctl create iamserviceaccount \
    --name ebs-csi-controller-sa \
    --namespace kube-system \
    --cluster my-cluster \
    --attach-policy-arn arn:aws:iam::111122223333:policy/AmazonEKS_EBS_CSI_Driver_Policy \
    --approve \
    --override-existing-serviceaccounts
```
