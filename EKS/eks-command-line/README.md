# We must have all set of permission

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

========================================
## Now we are starting from here
- aws configure list

-  eksctl create cluster \
   --name my-cluster \
   --region us-east-1 \ 
   --version 1.22 \
   --nodegroup-name demo-nodes \
   --node-type t2.micro \
   --nodes 2 \
   --nodes-min 2 \
   --nodes-max 2
