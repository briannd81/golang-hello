# Hello World App in GoLang

## Pre-requisites

1. You have an AWS ECR Private Registry setup
2. You have a role with permissions to upload image to ECR Private Registry
3. You have AWS CLI tool installed on your machine
4. You have ECR credential helper to push the image on your machine

Set ECR credential helper so that you can authenticate and upload the image

Note: the credentials are valid for 12 hours

$ aws ecr get-login-password | docker login --username AWS --password-stdin [AWS_ACCOUNT_ID].dkr.ecr.[REGION].amazonaws.com

Note: this demo uses the AWS ECR Private Registry, so remember to replace the values in [AWS_ACCOUNT_ID], [REGION], and [ECR_REPOSITORY].

## Build the Docker Image

$ cd build

$ docker build -t golang-hello:latest .

## Run/Test the Docker Container

Verify the app displays "Hello World" by running the container on your local machine

$ docker run golang-hello:latest

## Push the Image to ECR

1. In order for docker to push the image to ECR, the image needs proper tags

$ docker tag golang-hello:latest [AWS_ACCOUNT_ID].dkr.ecr.[REGION].amazonaws.com/[ECR_REPOSITORY]

2. Push the image

$ docker push [AWS_ACCOUNT_ID].dkr.ecr.[REGION].amazonaws.com/[ECR_REPOSITORY]

## Deploy the App to a Kubernetes Cluster using a Kubernetes manifest file

Assuming you have aws cli and the AWS credentials that have access to ecr on the same machine running kubectl

1. Create the docker registry credentials and save it to a Kubernetes secret. this is required to pull the image from ECR.

Note: the credentials are valid for 12 hours

$ kubectl create secret docker-registry ecr-pull-secret --docker-server=[AWS_ACCOUNT_ID].dkr.ecr.[REGION].amazonaws.com --docker-username=AWS --docker-password=$(aws ecr get-login-password)

2. Deploy the Kubernetes manifest file to create the pod

$ kubectl apply -f deploy/k8s-manifests/golang-hello.yaml

Note: since this container only prints "Hello World" and exit, you won't be able to interact with it.

How do you check that the container prints the "Hello World"?

$ kubectl logs pod/[POD-NAME]

## Deploy the App to AWS ECS using Cloudformation
