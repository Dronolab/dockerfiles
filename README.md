# PX4-ROS2-Humble

Here is the Dockerfile:

```Dockerfile
# ROS2 Humble Desktop Base Image
FROM osrf/ros:humble-desktop-full

RUN apt update && apt upgrade -y

# Installing requirements
RUN apt install ca-certificates gnupg lsb-core sudo wget python3-pip -y
RUN pip3 install --ignore-installed empy==3.3.4 pyros-genmsg kconfiglib jsonschema

# Install PX4-Autopilot
WORKDIR /
RUN git clone https://github.com/PX4/PX4-Autopilot.git --recursive
WORKDIR "/PX4-Autopilot"
RUN bash ./Tools/setup/ubuntu.sh
RUN make px4_sitl
WORKDIR /

# Install Micro-XRCE-DDS-Agent
RUN git clone https://github.com/eProsima/Micro-XRCE-DDS-Agent.git
WORKDIR "/Micro-XRCE-DDS-Agent"
RUN mkdir build
WORKDIR "/Micro-XRCE-DDS-Agent/build"
RUN cmake ..
RUN make
RUN make install
RUN ldconfig /usr/local/lib/

# Root directory is default workdir for the container
WORKDIR /
```
