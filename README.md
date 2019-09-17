# memory_leak

Description: a testbed for debugging memory leaks

<img src="https://picknik.ai/assets/images/logo.jpg" width="100">

Developed by Mike Lautman at [PickNik Consulting](http://picknik.ai/)

## Install

### Build from Source

1. Install [ROS melodic](http://wiki.ros.org/melodic/Installation/Ubuntu) if you are running on Ubuntu 16.04 or [ROS Melodic](http://wiki.ros.org/melodic/Installation/Ubuntu) for Ubuntu 18.04. This package primerily targets melodic

1. Install the following build tools:

        sudo apt-get install python-wstool python-catkin-tools

1. Re-use or create a catkin workspace:

        export CATKIN_WS=~/ws_catkin/
        mkdir -p $CATKIN_WS
        cd $CATKIN_WS

1. Download the required repositories and install any dependencies:

        git clone git@github.com:PickNikRobotics/memory_leak.git
        wstool init src
        wstool merge -t src memory_leak/memory_leak.rosinstall
        wstool update -t src
        rosdep install --from-paths src --ignore-src --rosdistro ${ROS_DISTRO}

1. Configure and build the workspace:

        catkin config --extend /opt/ros/${ROS_DISTRO} --cmake-args -DCMAKE_BUILD_TYPE=Debug
        catkin build

1. Source the workspace.

        source devel/setup.bash

## Developers: Quick update code repositories

To make sure you have the latest repos:

    cd $CATKIN_WS/src/memory_leak
    git checkout master
    git pull origin master
    cd ..
    wstool merge memory_leak/memory_leak.rosinstall
    wstool update
    rosdep install --from-paths . --ignore-src --rosdistro ${ROS_DISTRO}

## Run

Run `class_name_main`:

    roslaunch memory_leak class_name_main.launch

### Run with Debuging

Run `class_name_main` with GDB:

    roslaunch memory_leak class_name_main.launch debug:=true

Run `class_name_main` with Callgrind:

    roslaunch memory_leak class_name_main.launch callgrind:=true

Run `class_name_main` with Valgrind:

    roslaunch memory_leak class_name_main.launch valgrind:=true