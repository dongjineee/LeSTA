FROM annazabnus/ros-cuda:11.8-noetic as base

ARG DEBIAN_FRONTEND=noninteractive
ARG ROS_DISTRO=noetic
ENV ROS_DISTRO=${ROS_DISTRO}

# Curl
RUN apt-get update && apt-get install -y curl
RUN curl -I https://raw.githubusercontent.com

# install ros packages
RUN apt-get update && apt-get install -y --no-install-recommends \
    "ros-noetic-desktop-full=1.5.0-1*" \
    && rm -rf /var/lib/apt/lists/*

# install bootstrap tools
RUN apt-get update && apt-get install --no-install-recommends -y \
    build-essential \
    python3-rosdep \
    python3-rosinstall \
    python3-vcstools \
    && rm -rf /var/lib/apt/lists/*

# bootstrap rosdep
RUN rm -rf /etc/ros/rosdep/sources.list.d/20-default.list && rosdep init && \
  rosdep update --rosdistro $ROS_DISTRO
  
# install ros packages
RUN apt-get update && apt-get install -y --no-install-recommends \
    ros-noetic-ros-base=1.5.0-1* \
    && rm -rf /var/lib/apt/lists/*

# Install commonly-used development tools.
RUN apt-get update && apt-get install --yes \
    build-essential \
    clang-12 \
    clang-format-12 \
    cmake \
    g++ \
    gdb \
    git \
    nano \
    valgrind \
    vim

# Install commonly-used Python tools.
RUN apt-get update && apt-get install --yes \
    python-is-python3 \
    python3-catkin-tools \
    python3-pip

# Install commonly-used command-line tools.
RUN apt-get update && apt-get install --yes \
    curl \
    iproute2 \
    iputils-ping \
    less \
    mesa-utils \
    net-tools \
    rsync \
    software-properties-common \
    sudo \
    tmux \
    tree \
    unzip \
    usbutils \
    wget \
    zip \
    zsh

RUN apt-get update && apt-get install ros-noetic-grid-map -y
    
# Fix networkx compatibility for Python 3.8 before installing PyTorch
RUN pip install 'networkx<3.0'

# Pytorch   
RUN pip install torch==2.2.2 torchvision==0.17.2 torchaudio==2.2.2 --index-url https://download.pytorch.org/whl/cu118

# Install LibTorch (2.6.0 + CUDA 11.8)
RUN wget https://download.pytorch.org/libtorch/cu118/libtorch-cxx11-abi-shared-with-deps-2.6.0%2Bcu118.zip && \
    unzip libtorch-cxx11-abi-shared-with-deps-2.6.0+cu118.zip && \
    rm libtorch-cxx11-abi-shared-with-deps-2.6.0+cu118.zip && \
    mv libtorch /opt/libtorch
ENV TORCH_DIR=/libtorch/share/cmake/Torch
ENV LIBTORCH /opt/libtorch
ENV LD_LIBRARY_PATH /opt/libtorch/lib:$LD_LIBRARY_PATH
    
ENV CMAKE_PREFIX_PATH="/libtorch/share/cmake/Torch:${CMAKE_PREFIX_PATH}"

RUN mkdir -p /root/catkin_ws/src && \
    cd /root/catkin_ws/src && \
    echo "Create sim_pkg directory"
FROM base as dev
