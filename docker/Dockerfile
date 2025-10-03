# Select your ROS version or container to start from
FROM ubuntu:22.04
ENV DEBIAN_FRONTEND=noninteractive

# Some basic tools that you will include everytime
RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y  git \
    sudo \
    curl \
    software-properties-common \
    wget \
    tmux \
    tmuxinator \
    vim \
    xterm \
    curl \
    ssh \
    tree \
    python3-tk \
    nano \
    libtool \
    libtool-bin \
    htop \
    gdb \
    net-tools \
    build-essential \
    cmake \
    lsb-release \
    iputils-ping

# Enable scroll in tmux
RUN printf "set -g mouse on\n\
bind -n M-Left select-pane -L\n\
bind -n M-Right select-pane -R\n\
bind -n M-Up select-pane -U\n\
bind -n M-Down select-pane -D\n\
\n\
# split panes using | and -\n\
bind | split-window -h\n\
bind - split-window -v\n\
unbind '\"'\n\
unbind %%\n" > /root/.tmux.conf

# ARENA 5 dependencies
RUN apt-get install -y locales && locale-gen en_US en_US.UTF-8 && update-locale LANG=en_US.UTF-8 LC_ALL=en_US.UTF-8 && export LANG=en_US.UTF-8
RUN add-apt-repository universe
RUN apt-get update
RUN export ROS_APT_SOURCE_VERSION=$(curl -s https://api.github.com/repos/ros-infrastructure/ros-apt-source/releases/latest | grep -F "tag_name" | awk -F\" '{print $4}')
RUN curl -L -o /tmp/ros2-apt-source.deb "https://github.com/ros-infrastructure/ros-apt-source/releases/download/${ROS_APT_SOURCE_VERSION}/ros2-apt-source_${ROS_APT_SOURCE_VERSION}.$(. /etc/os-release && echo ${UBUNTU_CODENAME:-${VERSION_CODENAME}})_all.deb"
RUN dpkg -i /tmp/ros2-apt-source.deb
RUN rm /tmp/ros2-apt-source.deb
RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y python3-rosdep

WORKDIR /root/
ADD ./install.sh /root/install.sh
# RUN curl https://raw.githubusercontent.com/Arena-Rosnav/arena-rosnav/humble/installers/install.sh > install.sh
# RUN bash install.sh