FROM ros:foxy

RUN apt update && apt install -y --no-install-recommends\
  git \
  wget \
  build-essential \
  libboost-all-dev \
  python3-pip \
  python3-vcstool \
  python3-rosdep \
  python-is-python3 \
  python3-colcon-common-extensions \
	&& rm -rf /var/lib/apt/lists/

RUN mkdir -p /mros_reasoner_ws/src
WORKDIR /mros_reasoner/src

RUN git clone https://github.com/meta-control/mc_mros_reasoner.git
# RUN vcs import < mc_mros_reasoner/mros2_reasoner.rosinstall
#
# WORKDIR /mros_reasoner
# RUN [ "/bin/bash","-c","source /opt/ros/foxy/setup.bash \
#             && apt update && rosdep update \
#             && rosdep install --from-paths src --ignore-src -r -y \
#             && rm -rf /var/lib/apt/lists/"]

WORKDIR /mros_reasoner/src

RUN apt update && apt install -y --no-install-recommends\
  ros-foxy-urdf \
  ros-foxy-laser-geometry \
  ros-perception \
  ros-foxy-gazebo-ros \
  ros-foxy-gazebo-ros-pkgs \
  ros-foxy-test-msgs \
  ros-foxy-behaviortree-cpp-v3 \
  ros-foxy-map-msgs \
  ros-foxy-rviz-common \
  ros-foxy-rviz-default-plugins \
  ros-foxy-angles \
  ros-foxy-nav2-* \
  ros-foxy-rviz2 \
  && rm -rf /var/lib/apt/lists/

COPY dependencies.repos Pilot-URJC/dependencies.repos
COPY turtlebot3.repos Pilot-URJC/turtlebot3.repos

RUN vcs import < Pilot-URJC/dependencies.repos
RUN vcs import < Pilot-URJC/turtlebot3.repos

# RUN git clone -b metacontrol-test-integration https://github.com/MROS-RobMoSys-ITP/Pilot-URJC.git
COPY laser_resender Pilot-URJC/laser_resender/
COPY metacontroller_pilot Pilot-URJC/metacontroller_pilot/
COPY mros_contingencies_sim Pilot-URJC/mros_contingencies_sim/
COPY pilot_behavior Pilot-URJC/pilot_behavior/
COPY pilot_kobuki Pilot-URJC/pilot_kobuki/
COPY pilot_urjc_bringup Pilot-URJC/pilot_urjc_bringup/
COPY resources Pilot-URJC/resources/

WORKDIR /mros_reasoner
RUN [ "/bin/bash","-c","source /opt/ros/foxy/setup.bash \
            && apt update && rosdep update \
            && rosdep install --from-paths src --ignore-src -r -y \
            && rm -rf /var/lib/apt/lists/"]

RUN [ "/bin/bash","-c","source /opt/ros/foxy/setup.bash \
            && colcon build --symlink-install"]

ENV NVIDIA_VISIBLE_DEVICES \
   ${NVIDIA_VISIBLE_DEVICES:-all}
ENV NVIDIA_DRIVER_CAPABILITIES \
   ${NVIDIA_DRIVER_CAPABILITIES:+$NVIDIA_DRIVER_CAPABILITIES,}graphics

ENV GAZEBO_MODEL_PATH=$GAZEBO_MODEL_PATH:/mros_reasoner/src/turtlebot3/turtlebot3_simulations/turtlebot3_gazebo/models
ENV TURTLEBOT3_MODEL=${TB3_MODEL}

COPY docker/entrypoint.sh /
ENTRYPOINT ["/entrypoint.sh"]
