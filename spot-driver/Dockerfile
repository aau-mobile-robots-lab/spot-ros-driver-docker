FROM ros:melodic-ros-core

# Install the Boston Dynamics python driver
RUN apt-get update \
    && apt-get install -y python3-pip \
    && pip3 install bosdyn-client bosdyn-mission bosdyn-api bosdyn-core \
    && rm -rf /var/lib/apt/lists/*

# Get dependencies for the Clearpath ROS driver, get the code and build it.
ENV ROS_WS /root/catkin_ws
RUN mkdir -p ${ROS_WS}/src
WORKDIR ${ROS_WS}
RUN apt-get update \
    && apt-get install -y git wget python-rosdep bridge-utils python-catkin-tools\
    && pip3 install cython empy\
    && rosdep init \
    && rosdep fix-permissions \
    && wget https://raw.githubusercontent.com/clearpathrobotics/public-rosdistro/master/rosdep/50-clearpath.list -O /etc/ros/rosdep/sources.list.d/50-clearpath.list \
    && rosdep update \
    && git -C src clone https://github.com/clearpathrobotics/spot_ros.git \
    && git -C src clone https://github.com/ros/geometry2 --branch 0.6.5 \
    && rosdep install --from-paths src --ignore-src -y \
    && catkin config --extend /opt/ros/$ROS_DISTRO \
    && catkin build --cmake-args -DCMAKE_BUILD_TYPE=Release -DPYTHON_EXECUTABLE=/usr/bin/python3 -DPYTHON_INCLUDE_DIR=/usr/include/python3.6m -DPYTHON_LIBRARY=/usr/lib/x86_64-linux-gnu/libpython3.6m.so \
    && rm -rf /var/lib/apt/lists/*