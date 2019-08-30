FROM moveit/moveit:kinetic-source

MAINTAINER Yu Yan yu.yan@intel.com

SHELL ["/bin/bash", "-c"]

ENV ROS_DISTRO kinetic

RUN mv /etc/apt/sources.list.d/ros1-latest.list /etc/apt/sources.list.d/ros-latest.list

RUN apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' \ 
      --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654

RUN apt-get update && apt-get install -y \
      vim \
      ros-kinetic-gazebo* \
      ros-kinetic-ros-control \
      ros-kinetic-ros-controllers && \
      rm -rf /var/lib/apt/lists/*

ENV WS_DENSO /root/ws_denso
RUN mkdir -p $WS_DENSO/src
WORKDIR $WS_DENSO/src

RUN git clone https://github.com/RoboticsYY/denso_robot_ros.git && \
      git clone https://github.com/RoboticsYY/denso_cobotta_ros.git && \
      cd .. && \
      catkin config --extend /opt/ros/$ROS_DISTRO && \
      catkin build

WORKDIR $WS_DENSO
