#####################
# Base image #
#####################
FROM dongjineee/cuda_12_1_0-20_04-noetic:01 as base

# Labels
LABEL maintainer="dongjincho"
LABEL contact="d22g66@naver.com"
LABEL description="lesta"

ARG ROS_VERSION="noetic"

# Disable dialog frontend
ARG DEBIAN_FRONTEND=noninteractive

# Select shell
SHELL ["/bin/bash", "-c"]

# Install dependency ros_sim_pkg
RUN apt update && apt install -y \
    python3-catkin-tools && \
    apt clean && rm -rf /var/lib/apt/lists/*

# Create sim_pkg
RUN mkdir -p /root/catkin_ws/src && \
    cd /root/catkin_ws/src && \
    echo "Create sim_pkg directory"
# Define the dev stage for Docker Compose
FROM base as dev

# Set working directory
WORKDIR /root/catkin_ws

