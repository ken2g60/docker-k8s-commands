# Docker image is a read-only blueprint or template, while a Docker container is a live, running instance created from that blueprint.

# Docker commands

> docker run node # install latest node version
> docker ps -a # check for process
> docker run -it node # interactive mode
> docker run <docker-image-number>
> docker stop <docker-container-name>
> docker run -p 3000:80 <docker-image-number>
> docker run -p 8000:80 -d <docker-image-number> # deattach mode
> docker logs -f <docker-container-name>
> docker image inspect <docker-image-number>
> docker compose up | down

# kubl

> minikube start
> docker build -t kub-first-app .
> kubectl create deployment first-app --image=kub-first-app
> kubectl get deployments
> kubectl get pods
> kubectl delete deployment first-app
> minikube status

minikube start --driver=docker
minikube delete --all --purge

```
minikube start \
  --driver=docker \
  --cpus=4 \
  --memory=4096 \
  --alsologtostderr \
  --wait-timeout=10m
```

> docker tag <container-name> username/<container-name>
> docker build -t knightshell301/first-kub-app:latest .
> docker push knightshell301/first-kub-app:latest

# deployment of the container image from dockerhub

> kubectl create deployment first-app --image=knightshell301/first-kub-app
> kubectl get deployments

> kubectl get pods --field-selector status.phase!=Running | grep ImagePullBackOff
> kubectl describe pod first-app-b796466f4-8d2ff

kubectl create secret docker-registry regcred \
 --docker-server=https://index.docker.io/v1/ \
 --docker-username=<dockerhub-username> \
 --docker-password=<dockerhub-password> \
 --docker-email=<email>

# expose deployment

> kubectl expose deployment first-app --type=LoadBalancer --port=8080

# get service

> kubectl get services

# minikube dashboard

> minikube dashboard

> minikube service first-app

# updating deployment

> kubectl set image deployment/first-app <image-name>=<username>/<repo-storage-name>:latest

# rollback images and deployment

> kubectl rollout history deployment/first-app -revision=3

# undo

> kubectl rollout undo deployment/first-app --to-revision=3

# delete service

> kubectl delete service first-app
> kubectl delete deployment first-app

# delcarative commands

> kubectl apply -f=deployment.yml

# deleting resources from deployment.yml

> kubectl delete -f=deployment.yml -f=services.yml
