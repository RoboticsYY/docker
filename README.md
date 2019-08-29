# docker

Storage of Dockerfiles

## Setup display of docker container

```shell
./setup_docker_display.sh

docker run -t -i --rm -v /tmp/.X11-unix:/tmp/.X11-unix:rw -v /tmp/.docker.xauth:/tmp/.docker.xauth:rw -e XAUTHORITY=/tmp/.docker.xauth -e DISPLAY --name <Docker_Container_Name> <Docker_Image_Name> bash
```
