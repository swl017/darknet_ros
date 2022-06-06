# Build command: 
# docker build --tag ros_cude:18.04_11.0.3 .
# ROS + CUDA docker image for running darknet_ros
FROM nvidia/cuda:11.0.3-cudnn8-devel-ubuntu18.04

# INSTALL ROS MELODIC
# setup timezone
RUN echo 'Etc/UTC' > /etc/timezone && \
    ln -s /usr/share/zoneinfo/Etc/UTC /etc/localtime && \
    apt-get update && \
    apt-get install -q -y --no-install-recommends tzdata && \
    rm -rf /var/lib/apt/lists/*

# install packages
RUN apt-get update && apt-get install -q -y --no-install-recommends \
    dirmngr \
    gnupg2 \
    && rm -rf /var/lib/apt/lists/*

# setup sources.list
RUN echo "deb http://packages.ros.org/ros/ubuntu bionic main" > /etc/apt/sources.list.d/ros1-latest.list

# setup keys
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654

# setup environment
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8

ENV ROS_DISTRO melodic

# install ros packages
RUN apt-get update && apt-get install -y --no-install-recommends \
    ros-melodic-ros-core=1.4.1-0* \
    && rm -rf /var/lib/apt/lists/*

# INSTALL ROS MELODIC END

# Set user
RUN ["useradd", "-m", "usrg"]

RUN apt-get update && apt-get install -y sudo
RUN echo "usrg:'" | chpasswd
RUN adduser usrg sudo
RUN echo 'usrg ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
# CMD ["./opt/ros/melodic/setup.bash"]

# install dependencies
RUN apt-get install -y \
        libx11-dev \
        libopencv-dev \
        libxext-dev \
        libxt-dev \
        ros-melodic-cv-bridge \
        ros-melodic-image-transport \
        wget \
        iputils-ping \
        net-tools

USER usrg
WORKDIR /home/usrg

# setup entrypoint
COPY ./ros_entrypoint.sh /

ENTRYPOINT ["/ros_entrypoint.sh"]