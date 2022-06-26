# BDP2-Review

## Midterm Review repository for the BDP2 Course

This repository contains an exercise dealing with the following technologies:
- `Dockerfile` to create Docker images;
- `git` and _GitHub_ for version control;
- _DockerHub_ for automated builds of Docker images;
- Docker volumes and bind mounts;
- Docker process monitoring and loggin;
- `docker-compose` to create application stacks;
- X.509 authentication via `https`

### Creating a Docker image using a Dockerfile

In order to create a Docker image containing Jupyter notebooks and access to the Redis module, a Dockerfile was used to expand the existing image `jupyter/minimal-notebook`.
The Redis module was installed adding the following line to the Dockerfile:
```
RUN pip install redis
```
Using a GitHub Action, the Dockerfile is automatically used to build the corresponding image in the `marcolorusso/jupyreview` repository hosted in DockerHub.

To run the container, run the following command:
```
docker run -d --rm --name my_jupyter -v ~/review:/home/jovyan -p 80:8888 --network bdp2-net -e JUPYTER_ENABLE_LAB=yes -e JUPYTER_TOKEN="<insert-jupyter-token>" --user root -e CHOWN_HOME=yes -e CHOWN_HOME_OPTS="-R" marcolorusso/jupyreview
```

Some notes on this command:
- the `-d --rm` flags make the container run in the background and delete it after it stops;
- the port 8888 in the container is mapped to port 80 on the docker host using `-p`;
- with `--network` a custom network bridge is specified

### Using `docker-compose` to create an application stack

Using `docker-compose` it is possible to create multiple containers linked together to provide a multi-container service, i.e. an _application stacks_.
`docker-compose` works by parsing a text file, written in the YAML language. This file, which can be found in the [docker/](https://github.com/DrWatt/BDP2-Review/tree/main/docker)  folder, defines how our application stack is structured.

This file describes three services:
• The Jupyter custom image created using the `Dockerfile`.
• The Redis image.
• A Portainer image useful for managing containers via a web interface.

Also volume bindings to the `work` directory are included, together with the creation of a Docker volume mounted on the Portainer container. 
Two network bridges are declared in the file: a _backend_, which links the containers, and a _frontend_ used to access the containers from the outside world.
The _jupyter-hub_ interface is accesible using the port 80, while Portainer via port 443. 
In order to build and run the stack, the following command has to be issued:
```
docker-compose up -d
```
where the `-d` flag is used to run the stack in the background.
Finally
```
docker-compose down
```
can be used to stop the application.



