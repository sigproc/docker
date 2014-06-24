# Based on the Docker file found at:
#
#	https://github.com/opencog/ros/blob/master/hydro/Dockerfile
#
# Original maintainer Mandeep Singh Bhatia "mandeep.singh.bhatia@gmail.com"
# Original maintainer David Hart "dhart@opencog.org"

FROM ubuntu:12.04

MAINTAINER Rich Wareham <rjw57@cam.ac.uk>

# Use the "noninteractive" debconf frontend since we're installing in a non-interactive way.
ENV DEBIAN_FRONTEND noninteractive

# Make sure we have the latest package sources
RUN apt-get -y update

# Install package containing the CLI tools we'll be using. N.B. after "precise"
# the add-apt-repository command moved to software-properties-common.
RUN apt-get -y install apt-utils
RUN apt-get -y install wget python-software-properties sudo

# Below we're using https so make sure the ca-certificates are installed
RUN apt-get -y install ca-certificates

# Install the ROS package signing key and update the source.list
RUN wget https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -O - | apt-key add -
RUN add-apt-repository -y "deb http://packages.ros.org/ros/ubuntu precise main"

# Fetch ROS package repo details
RUN apt-get -y update

# Install ROS
RUN apt-get -y install ros-hydro-desktop-full

# Install additional useful packages
RUN apt-get -y install bash-completion git build-essential vim

# Initialise rosdep
RUN rosdep init

# Now create the ros user itself
RUN adduser --gecos "ROS User" --disabled-password ros

# And, as that user...
USER ros

# HOME needs to be set explicitly. Without it, the HOME environment variable is
# set to "/"
RUN HOME=/home/ros rosdep update

# Create a ROS workspace for the ROS user.
RUN mkdir -p /home/ros/workspace/src
RUN /bin/bash -c '. /opt/ros/hydro/setup.bash; catkin_init_workspace /home/ros/workspace/src'
RUN /bin/bash -c '. /opt/ros/hydro/setup.bash; cd /home/ros/workspace; catkin_make'
RUN echo "source ~/workspace/devel/setup.bash" >> /home/ros/.bashrc
