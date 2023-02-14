Hello World App in GoLang

Build the Docker Image
- Build with docker
$ cd build
$ docker build -t golang-hello:1.0 .
- Build with podman
$ cd build
$ podman build -t golang-hello:1.0 .

Run/Test the Docker Container
Verify the app displays "Hello World" by running the container on your local machine
- Test with docker
$ docker run golang-hello:1.0
- Test with podman
$ podman run golang-hello:1.0


Push the Image to a Container Registry

Deploy the App to AWS ECS

Deploy the App to Kubernetes Cluster
