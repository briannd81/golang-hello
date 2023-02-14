Hello World App in GoLang

Note: this demo uses AWS ECR Private Registry, so remember to replace the values in [AWS_ACCOUNT_ID], [REGION], and [ECR_REPOSITORY].

Build the Docker Image

$ cd build

$ docker build -t golang-hello:latest .

Run/Test the Docker Container

Verify the app displays "Hello World" by running the container on your local machine

$ docker run golang-hello:latest

Push the Image to a Private Image Registry

In this demo, the image is hosted in the AWS ECR Private Registry to demonstrate how to handle the registry credentials securely in Cloudformation and Kubernetes manifest files.

In order for docker to push the image to ECR, the image needs proper tags

$ docker tag golang-hello:latest [AWS_ACCOUNT_ID].dkr.ecr.[REGION].amazonaws.com/[ECR_REPOSITORY]

Set ECR credential helper so that you can authenticate and upload the image

Note: the credentials are valid for 12 hours

$ aws ecr get-login-password | docker login --username AWS --password-stdin [AWS_ACCOUNT_ID].dkr.ecr.[REGION].amazonaws.com

Using AWS CLI, push the image to ECR

$ docker push [AWS_ACCOUNT_ID].dkr.ecr.[REGION].amazonaws.com/[ECR_REPOSITORY]

Deploy the App to AWS ECS using Cloudformation

Deploy the App to a Kubernetes Cluster using a Kubernetes manifest file

Assuming you have aws cli and the AWS credentials that have access to ecr on the same machine running kubectl

1. Create the credentials to pull the image from the ECR Private Registry
Note: the credentials are valid for 12 hours

$ kubectl create secret docker-registry ecr-pull-secret --docker-server=[AWS_ACCOUNT_ID].dkr.ecr.[REGION].amazonaws.com --docker-username=AWS --docker-password=$(aws ecr get-login-password)

2. Deploy the Kubernetes manifest file to create the pod

$ kubectl apply -f deploy/k8s-manifests/golang-hello.yaml

Note: since this container only prints "Hello World" and exit, you won't be able to interact with it.

How do you check that the container prints the "Hello World"?

$ kubectl logs pod/[POD-NAME]
