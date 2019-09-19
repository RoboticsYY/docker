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

ENV DEPEND /root/depends
RUN mkdir -p $DEPEND

# install openvino 
WORKDIR $DEPEND
RUN mkdir -p openvino_binart && cd openvino_binart && \
    apt-get update && apt-get install -y cpio && \
      rm -rf /var/lib/apt/lists/*
# wget openvino_R5
RUN wget -c http://registrationcenter-download.intel.com/akdlm/irc_nas/15078/l_openvino_toolkit_p_2018.5.455.tgz && \
    tar -xvf l_openvino_toolkit_p_2018.5.455.tgz &&rm l_openvino_toolkit_p_2018.5.455.tgz && \
    cd l_openvino_toolkit_p_2018.5.455 && \
    ./install_cv_sdk_dependencies.sh && \
    sed -i 's/ACCEPT_EULA=decline/ACCEPT_EULA=accept/g' silent.cfg && \
    ./install.sh --silent silent.cfg
# install dependencies
RUN cd /opt/intel/computer_vision_sdk/install_dependencies && \
    ./install_NEO_OCL_driver.sh

# install gpg
WORKDIR $DEPEND
RUN git clone --depth=1 https://github.com/atenpas/gpg.git && \
    cd gpg && \
    mkdir build && cd build && \
    cmake .. && make && make install && \
    ldconfig

ENV WS_DENSO /root/ws_denso

WORKDIR $WS_DENSO
RUN . /opt/ros/$ROS_DISTRO/setup.bash && \  
    . /opt/intel/computer_vision_sdk/bin/setupvars.sh && \
      echo $InferenceEngine_DIR && \
      mkdir src && cd src && \
      git clone --depth=1 https://github.com/RoboticsYY/denso_robot_ros.git && \
      git clone --depth=1 https://github.com/RoboticsYY/denso_cobotta_ros.git && \
      git clone --depth=1 https://github.com/RoboticsYY/cobotta_pick_place.git && \
      git clone --depth=1 https://github.com/atenpas/gpd.git && \
      cd .. && \
      catkin config --extend /root/ws_moveit/install \ 
                    --cmake-args -DUSE_CAFFE=OFF -DUSE_OPENVINO=ON && \
      catkin build

WORKDIR $WS_DENSO
