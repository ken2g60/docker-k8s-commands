# Docker And Kubernetes Command Notes

This project is a quick reference for common Docker, Docker Compose, Minikube, and Kubernetes commands.

## Docker Basics

A Docker image is a read-only blueprint or template. A Docker container is a live instance created from that image.

## Docker Commands

| Command | Description |
| --- | --- |
| `docker run node` | Downloads the latest `node` image if needed and starts a container from it. |
| `docker ps -a` | Lists all containers, including running and stopped containers. |
| `docker run -it node` | Starts a Node container in interactive terminal mode. |
| `docker run <docker-image-number>` | Runs a container from the specified image ID. |
| `docker stop <docker-container-name>` | Stops a running container by name or container ID. |
| `docker run -p 3000:80 <docker-image-number>` | Runs a container and maps host port `3000` to container port `80`. |
| `docker run -p 8000:80 -d <docker-image-number>` | Runs a container in detached mode and maps host port `8000` to container port `80`. |
| `docker logs -f <docker-container-name>` | Streams logs from a container and follows new log output. |
| `docker image inspect <docker-image-number>` | Shows detailed metadata for a Docker image. |
| `docker compose up` | Starts services defined in a Compose file. |
| `docker compose down` | Stops and removes services, networks, and containers created by Compose. |

## Minikube Commands

| Command | Description |
| --- | --- |
| `minikube start` | Starts a local Kubernetes cluster. |
| `minikube start --driver=docker` | Starts Minikube using Docker as the virtualization driver. |
| `minikube delete --all --purge` | Deletes all Minikube clusters and purges related files. |
| `minikube status` | Shows the status of the local Minikube cluster. |
| `minikube dashboard` | Opens the Kubernetes dashboard for the Minikube cluster. |
| `minikube service first-app` | Opens or shows the URL for the `first-app` service. |

### Minikube Start With Options

```sh
minikube start \
  --driver=docker \
  --cpus=4 \
  --memory=4096 \
  --alsologtostderr \
  --wait-timeout=10m
```

This starts Minikube with Docker as the driver, 4 CPUs, 4096 MB of memory, extra logging, and a 10 minute wait timeout.

## Docker Image Build And Push

| Command | Description |
| --- | --- |
| `docker build -t kub-first-app .` | Builds a Docker image from the current directory and tags it as `kub-first-app`. |
| `docker tag <container-name> username/<container-name>` | Adds a Docker Hub style tag to an existing image. |
| `docker build -t knightshell301/first-kub-app:latest .` | Builds the current directory as an image tagged for the `knightshell301/first-kub-app` Docker Hub repository. |
| `docker push knightshell301/first-kub-app:latest` | Pushes the tagged image to Docker Hub. |

## Kubernetes Deployment Commands

| Command | Description |
| --- | --- |
| `kubectl create deployment first-app --image=kub-first-app` | Creates a deployment named `first-app` from the local `kub-first-app` image. |
| `kubectl create deployment first-app --image=knightshell301/first-kub-app` | Creates a deployment using the image from Docker Hub. |
| `kubectl get deployments` | Lists Kubernetes deployments. |
| `kubectl get pods` | Lists Kubernetes pods. |
| `kubectl delete deployment first-app` | Deletes the `first-app` deployment. |
| `kubectl describe pod first-app-b796466f4-8d2ff` | Shows detailed information and events for the named pod. |
| `kubectl get pods --field-selector status.phase!=Running` | Lists pods that are not in the `Running` phase. |

To filter for pods with image pull issues:

```sh
kubectl get pods --field-selector status.phase!=Running | grep ImagePullBackOff
```

## Docker Hub Pull Secret

```sh
kubectl create secret docker-registry regcred \
  --docker-server=https://index.docker.io/v1/ \
  --docker-username=<dockerhub-username> \
  --docker-password=<dockerhub-password> \
  --docker-email=<email>
```

Creates a Kubernetes image pull secret named `regcred` for authenticating with Docker Hub.

## Exposing And Viewing Services

| Command | Description |
| --- | --- |
| `kubectl expose deployment first-app --type=LoadBalancer --port=8080` | Exposes the `first-app` deployment as a LoadBalancer service on port `8080`. |
| `kubectl get services` | Lists Kubernetes services. |
| `kubectl delete service first-app` | Deletes the `first-app` service. |

## Updating And Rolling Back Deployments

| Command | Description |
| --- | --- |
| `kubectl set image deployment/first-app <image-name>=<username>/<repo-storage-name>:latest` | Updates the container image used by the `first-app` deployment. |
| `kubectl rollout history deployment/first-app --revision=3` | Shows rollout details for revision `3` of the deployment. |
| `kubectl rollout undo deployment/first-app --to-revision=3` | Rolls the deployment back to revision `3`. |

## Declarative Kubernetes Commands

| Command | Description |
| --- | --- |
| `kubectl apply -f=deployment.yml` | Creates or updates resources defined in `deployment.yml`. |
| `kubectl delete -f=deployment.yml -f=services.yml` | Deletes resources defined in `deployment.yml` and `services.yml`. |
