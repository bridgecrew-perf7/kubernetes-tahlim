# We must have all set of permission

## Step-00: Introduction
- Install AWS CLI
- Install kubectl CLI
- Install eksctl CLI

## Step-01: Install the AWS CLI on Linux

- curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
- unzip awscliv2.zip
- sudo ./aws/install
- ./aws/install -i /usr/local/aws-cli -b /usr/local/bin
- aws --version

## Step-02: aws configure
- aws configure

Note: (Access and Secrete key should be same the user which you had created eks cluster) 

## Step-03: Install kubectl

- curl -o kubectl https://s3.us-west-2.amazonaws.com/amazon-eks/1.22.4/2021-07-05/bin/linux/amd64/kubectl
- chmod +x ./kubectl
- mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$PATH:$HOME/bin
- echo 'export PATH=$PATH:$HOME/bin' >> ~/.bashrc
- kubectl
- kubectl version --short --client

## Step-04: Install aws-iam-authenticator for eks
- curl -o aws-iam-authenticator https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/aws-iam-authenticator
- chmod +x ./aws-iam-authenticator &&  mkdir -p $HOME/bin && cp ./aws-iam-authenticator $HOME/bin/aws-iam-authenticator && export PATH=$PATH:$HOME/bin && echo 'export PATH=$PATH:$HOME/bin' >> ~/.bashrc
- aws-iam-authenticator help
- kubectl cluster-info

==============================================

# Now We are going to create EKS Cluster & Node Groups from here
- aws configure lis

## Step-01: Create EKS Cluster using eksctl
- It will take 15 to 20 minutes to create the Cluster Control Plane 
```
# Create Cluster
eksctl create cluster --name=eksdemo1 \
                      --region=us-east-1 \
                      --zones=us-east-1a,us-east-1b \
                      --without-nodegroup 

# Get List of clusters
eksctl get cluster                  
```

## Step-02: Create & Associate IAM OIDC Provider for our EKS Cluster

```                   
# Template
eksctl utils associate-iam-oidc-provider \
    --region region-code \
    --cluster <cluter-name> \
    --approve

# Replace with region & cluster name
eksctl utils associate-iam-oidc-provider \
    --region us-east-1 \
    --cluster eksdemo1 \
    --approve
```


## Step-03: Create EC2 Keypair

## Step-04: Create Node Group with additional Add-Ons in Public Subnets
- These add-ons will create the respective IAM policies for us automatically within our Node Group role.
 ```
# Create Public Node Group   
eksctl create nodegroup --cluster=eksdemo1 \
                        --region=us-east-1 \
                        --name=eksdemo1-ng-public1 \
                        --node-type=t3.medium \
                        --nodes=2 \
                        --nodes-min=2 \
                        --nodes-max=4 \
                        --node-volume-size=20 \
                        --ssh-access \
                        --ssh-public-key=kube-demo \
                        --managed \
                        --asg-access \
                        --external-dns-access \
                        --full-ecr-access \
                        --appmesh-access \
                        --alb-ingress-access 
```

## Step-05: Verify Cluster & Nodes

```
# List EKS clusters
eksctl get cluster

# List NodeGroups in a cluster
eksctl get nodegroup --cluster=<clusterName>

# List Nodes in current kubernetes cluster
kubectl get nodes -o wide

# Our kubectl context should be automatically changed to new cluster
kubectl config view --minify
```

### Login to Worker Node using Keypai kube-demo
- Login to worker node
```
# For MAC or Linux or Windows10
ssh -i kube-demo.pem ec2-user@<Public-IP-of-Worker-Node>

```

## Step-06: Update Worker Nodes Security Group to allow all traffic
- We need to allow `All Traffic` on worker node security group
