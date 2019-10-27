FROM acutronicrobotics/moveit2:dashing-alpha

LABEL MAINTAINER = 'Yu Yan (yu.yan@intel.com)'

ARG http_proxy

RUN apt-key adv --keyserver keyserver.ubuntu.com \
      --keyserver-options http-proxy=$http_proxy \
      --recv-keys F42ED6FBAB17C654

RUN apt-get update && apt-get install -y \
      vim && \
      rm -rf /var/lib/apt/lists/*