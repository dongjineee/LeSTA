FROM nvidia/cuda:12.1.0-devel-ubuntu20.04 AS dev

ARG ROS_VERSION=noetic
ARG DEBIAN_FRONTEND=noninteractive
SHELL ["/bin/bash", "-c"]

################# ROS Install #################
RUN apt-get update && apt-get install -y --no-install-recommends \
      curl gnupg lsb-release git build-essential nano unzip wget \
 && curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key \
      | gpg --dearmor -o /usr/share/keyrings/ros-archive-keyring.gpg \
 && echo "deb [arch=amd64 signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] \
      http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" \
      > /etc/apt/sources.list.d/ros-latest.list \
 && apt-get update && apt-get install -y --no-install-recommends \
      ros-${ROS_VERSION}-desktop-full \
      python3-pip python3-rosdep python3-catkin-tools \
 && rosdep init && rosdep update \
 && echo "source /opt/ros/${ROS_VERSION}/setup.bash" >> /root/.bashrc \
 && apt-get clean && rm -rf /var/lib/apt/lists/*

#################  LibTorch 2.4.1 (CPU) #################
RUN wget -q https://download.pytorch.org/libtorch/cpu/libtorch-cxx11-abi-shared-with-deps-2.4.1%2Bcpu.zip \
      -O /tmp/libtorch.zip \
 && unzip -q /tmp/libtorch.zip -d /opt && rm /tmp/libtorch.zip \
 && echo "export CMAKE_PREFIX_PATH=/opt/libtorch:\$CMAKE_PREFIX_PATH" >> /root/.bashrc \
 && echo "export LD_LIBRARY_PATH=/opt/libtorch/lib:\$LD_LIBRARY_PATH" >> /root/.bashrc

################# Depen #################
RUN apt-get update && apt-get install -y --no-install-recommends \
      libpcl-dev libeigen3-dev \
      ros-noetic-grid-map ros-noetic-grid-map-core \
      python3-empy \
 && apt-get clean && rm -rf /var/lib/apt/lists/*

################# Python_Depen #################
RUN pip install --no-cache-dir --extra-index-url https://download.pytorch.org/whl/cpu \
      torch==2.4.1+cpu torchvision==0.19.1+cpu "networkx<3.2" tensorboard

################# workspace #################
RUN mkdir -p /root/lesta_ws/src
WORKDIR /root/lesta_ws

RUN /bin/bash -c "source /opt/ros/${ROS_VERSION}/setup.bash && \
                  catkin config --extend /opt/ros/${ROS_VERSION}"

ENV USER=root \
    HOME=/root \
    CMAKE_PREFIX_PATH=/opt/libtorch:$CMAKE_PREFIX_PATH \
    LD_LIBRARY_PATH=/opt/libtorch/lib:$LD_LIBRARY_PATH

RUN echo 'export PYTHONPATH=/root/lesta_ws/src/LeSTA/pylesta:$PYTHONPATH' >> /root/.bashrc \
 && echo 'source /root/lesta_ws/devel/setup.bash' >> /root/.bashrc


WORKDIR /root/lesta_ws
