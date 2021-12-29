# cheatsheet-docker
Referencing the most useful and/or common commands

### List Docker CLI commands
```
docker
docker container --help
```

#### Display Docker version and info
docker --version
docker version
docker info

## Execute Docker image
docker run hello-world

## List Docker images
docker image ls

## List Docker containers (running, all, all in quiet mode)
docker container ls
docker container ls --all
docker container ls -aq

## All relevant and useful Docker commands related to containers and images
docker build -t friendlyhello .                             # Create image using this directory's Dockerfile
docker run -p 4000:80 friendlyhello                         # Run "friendlyhello" mapping port 4000 to 80
docker run -d -p 4000:80 friendlyhello                      # Same thing, but in detached mode
docker container ls                                         # List all running containers
docker container ls -a                                      # List all containers, even those not running
docker container stop <hash>                                # Gracefully stop the specified container
docker container kill <hash>                                # Force shutdown of the specified container
docker container rm <hash>                                  # Remove specified container from this machine
docker container rm $(docker container ls -a -q)            # Remove all containers
docker image ls -a                                          # List all images on this machine
docker image rm <image id>                                  # Remove specified image from this machine
docker image rm $(docker image ls -a -q)                    # Remove all images from this machine
docker login                                                # Log in this CLI session using your Docker credentials
docker tag <image> username/repository:tag                  # Tag <image> for upload to registry
docker push username/repository:tag                         # Upload tagged image to registry
docker run username/repository:tag                          # Run image from a registry

docker-compose down -v                                      # Stop and remove all containers and volumes related the to .yml file
docker rmi -f $(docker images -a -q)                        # Delete all images on the host machine
docker volume rm $(docker volume ls -q)                     # Delete all volumes on the host machine
docker container prune                                      # Delete all containers on the host machine

## To recap, while typing docker run is simple enough, the true implementation of a container in production is running it as a service. Services codify a container’s behavior in a Compose file, and this file can be used to scale, limit, and redeploy our app. Changes to the service can be applied in place, as it runs, using the same command that launched the service: docker stack deploy.

## Some commands to explore at this stage:
docker stack ls                                             # List stacks or apps
docker stack deploy -c <composefile> <appname>              # Run the specified Compose file
docker service ls                                           # List running services associated with an app
docker service ps <service>                                 # List tasks associated with an app
docker inspect <task or container>                          # Inspect task or container
docker container ls -q                                      # List container IDs
docker stack rm <appname>                                   # Tear down an application
docker swarm leave --force                                  # Take down a single node swarm from the manager

## NOTE: In order to create a docker machine without the VT-X/AMD-v enabled check.
docker-machine create `name` --virtualbox-no-vtx-check
docker-machine create -d virtualbox --virtualbox-no-vtx-check `name`