

Task 2: Creating the Amazon EKS cluster
In this task, you use the eksctl command to create an Amazon EKS cluster and a dedicated virtual private cloud (VPC) in the lab account. To learn more about the eksctl command, see:

Getting started with Amazon EKS – eksctl
Introduction to eksctl
 Command: To create th
 
instalas cliente para administrar cluster 

eksctl create cluster \
--name eks-lab-cluster \
--nodegroup-name worknodes-1 \
--node-type t3.medium \
--nodes 2 \
--nodes-min 1 \
--nodes-max 4 \
--managed \
--version 1.29 \
--region ${AWS_DEFAULT_REGION}

 ## buildeas las imagenes, en base a los dockerfiles
 
 ## Creas los repo de registry por cada modulo:
 AWSLabsUser-rvVb8WNYxuZM4ftMDTVz7w:~/environment/eksLabRepo/sidecar (main) $ aws ecr create-repository \
>  --repository-name website \
>  --region ${AWS_DEFAULT_REGION}
{
    "repository": {
        "repositoryArn": "arn:aws:ecr:us-west-2:180019741541:repository/website",
        "registryId": "180019741541",
        "repositoryName": "website",
        "repositoryUri": "180019741541.dkr.ecr.us-west-2.amazonaws.com/website",
        "createdAt": "2024-08-01T11:10:12.754000+00:00",
        "imageTagMutability": "MUTABLE",
        "imageScanningConfiguration": {
            "scanOnPush": false
        },
		
 
 ## Seteas variables
 
 ## Te autenticas en el repo
 
 aws ecr get-login-password \
--region ${AWS_DEFAULT_REGION} \
 | docker login \
 --username AWS \
 --password-stdin $ACCOUNT_NUMBER.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com

## Tageas las imagenes y las subis al registry 
docker tag website:latest $ECR_REPO_URI_WEBSITE:latest

Tag
REPOSITORY                                             TAG       IMAGE ID       CREATED         SIZE
sidecar                                                latest    46992092a570   5 minutes ago   182MB
180019741541.dkr.ecr.us-west-2.amazonaws.com/website   latest    b2ec37fb674e   6 minutes ago   217MB
website                                                latest    b2ec37fb674e   6 minutes ago   217MB

Push
docker push $ECR_REPO_URI_WEBSITE:latest


##

Task 5: Authenticating to the Amazon EKS cluster
In this task, you authenticate the AWS Cloud9 session with the Amazon EKS cluster.

 Command: To set up the environment, run the following commands:

echo "export AWS_DEFAULT_REGION=${AWS_DEFAULT_REGION}" >> ~/.bash_profile
source ~/.bash_profile
aws configure set default.region $AWS_DEFAULT_REGION



## Helm y se instala aws-load-balancer

