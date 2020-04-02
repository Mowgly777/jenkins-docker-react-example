# Jenkins

## Setting up docker

1. `docker network create jenkins` - create a bridge network in docker
2. `docker volume create jenkins-docker-certs` && `docker volume create jenkins-data` - No idea what this is doing
3. For an explanation of the below 2 , go to: <https://jenkins.io/doc/book/installing/#installing-docker>

```
docker container run --name jenkins-docker --detach \
  --privileged --network jenkins --network-alias docker \
  --env DOCKER_TLS_CERTDIR=/certs \
  --volume jenkins-docker-certs:/certs/client \
  --volume jenkins-data:/var/jenkins_home \
  --publish 2376:2376 docker:dind
```

&&

```
docker container run --name jenkins-blueocean --detach \
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  --volume $HOME:/home \  
  --publish 8080:8080 --publish 50000:50000 jenkinsci/blueocean
```

## Useful Docker commands

1. To execute bash within a docker container:
`docker container exec -it jenkins-blueocean bash`
(^p + ^q) to exit
2. To view the logs of a docker container:
`docker container logs Jenkins-blueocean`
3. The Jenkins master key is located in the docker container at:
`cat /var/jenkins_home/secrets/initialAdminPassword`
