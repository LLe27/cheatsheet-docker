# hints-docker
Referencing the most useful and/or common commands

### List Docker CLI commands
```
docker
docker container --help
```

### Display Docker version and info
```
docker --version
docker version
docker info
```

### Execute Docker image
```
docker run hello-world
```

### List Docker images
```
docker image ls
```

### List Docker containers (running, all, all in quiet mode)
docker container ls
docker container ls --all
docker container ls -aq

### All relevant and useful Docker commands related to containers and images
```
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
docker container prune                                      # Delete all containers on the host machine
docker volume prune                                         # Delete all volumes on the host machine
```

> To recap, while typing docker run is simple enough, the true implementation of a container in production is running it as a service. Services codify a container’s behavior in a Compose file, and this file can be used to scale, limit, and redeploy our app. Changes to the service can be applied in place, as it runs, using the same command that launched the service: docker stack deploy.

### Some commands to explore at this stage:
```
docker stack ls                                             # List stacks or apps
docker stack deploy -c <composefile> <appname>              # Run the specified Compose file
docker service ls                                           # List running services associated with an app
docker service ps <service>                                 # List tasks associated with an app
docker inspect <task or container>                          # Inspect task or container
docker container ls -q                                      # List container IDs
docker stack rm <appname>                                   # Tear down an application
docker swarm leave --force                                  # Take down a single node swarm from the manager
```
### NOTE: In order to create a docker machine without the VT-X/AMD-v enabled check.
```
docker-machine create `name` --virtualbox-no-vtx-check
docker-machine create -d virtualbox --virtualbox-no-vtx-check `name`
```

# 8-best-practices-docker
- Improve security.
- Optimize docker image size.
- Cleaner and more maintainable Dockerfile.

1. Use Official Docker Images as Base Images.
    - Cleaner Dockerfile.
    - Official Docker image.
    - Verified and already built with best practices.
2. Use specific docker image versions.
    - [ ] Do not use a random latest tag.
        - You might get different docker image versions.
        - Might break stuff.
        - Latest tag is unpredictatable.
    - [x] Fixate the version.
        - The more specific, the better.
3. Use Small-Size Official Images
    - Less storage space.
    - Transfer images faster.
    - Use image based on a __leaner and and smaller OS distro__.
4. Optimize Caching Image Layers.
    - What are __Image Layers__?
        - Docker images are built based on a Dockerfile.
        - Each command creates an image layer.
    - What does "__Caching__ an __Image Layer__" mean?
        - Docker caches each layer, saved on local filesystem.
        - If nothing has changed in a layer (or any layers preceding it), it will be __re-used from cache__.
        - __Advantages__ of Cached Image Layers:
            - Faster image building.
            - Downloading only added layers.
        - Why isn't it used from cache?
            - Once a layer changes, __all the following layers are re-created__ as well.
    - __Order Dockerfile commands from least to most frequently changing.__
        - Take advantage of caching.
5. Use .dockerignore file
    - Reduce image size.
    - Prevent unintended secrets exposure.
6. Make us of "__Multi-Stage Builds__"
7. Use the Least Privileged User.
    - Create a dedicated user and group.
    - Don't forget to set required permissions.
    - Change to non-root user with __USER__ directive.
8. Scan your images for Security Vulnerabilities.
    - Scan using __CLI__: ```docker scan {{ image name }}```
    - Scan using __Docker Hub__.
    - Integrate in __CI/CD__.