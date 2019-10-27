FROM ros:melodic-ros-base

LABEL MAINTAINER = 'Yu Yan (yu.yan@intel.com)'

SHELL ["/bin/bash", "-c"]

WORKDIR /root/depends
RUN mkdir opencv_install && cd opencv_install && \
      git clone -b 3.4.5 --depth 1 https://github.com/opencv/opencv.git && \
      git clone -b 3.4.5 --depth 1 https://github.com/opencv/opencv_contrib.git

RUN cd opencv_install/opencv && mkdir build && cd build && \
      cmake -D CMAKE_BUILD_TYPE=RELEASE \
            -D WITH_IPP=OFF \
            -D CMAKE_INSTALL_PREFIX=/usr/local \
            -D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules \
            .. && \
      make -j $(nproc) && make install

ENV ROS_DISTRO melodic

ARG HTTP_PROXY

RUN apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' \ 
      --keyserver-options http-proxy=${HTTP_PROXY} \
      --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654 && \
      apt-get update && apt-get upgrade -y

RUN apt-get install -y python-catkin-tools \ 
      ros-melodic-rviz ros-melodic-rqt \
      ros-melodic-rqt-image-view ros-melodic-rqt-robot-dashboard \
      ros-melodic-cv-bridge

WORKDIR /root/ws_handeye
RUN source /opt/ros/melodic/setup.bash && \
      mkdir src && cd src && \
      git clone --depth 1 https://github.com/crigroup/handeye.git && \
      git clone --depth 1 https://github.com/crigroup/criutils.git && \
      git clone --depth 1 https://github.com/crigroup/baldor.git && \
      cd .. && catkin build
