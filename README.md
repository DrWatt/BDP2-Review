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



