# memory_leak

Description: a testbed for finding memory leaks

<img src="https://picknik.ai/assets/images/logo.jpg" width="100">

Developed by Mike Lautman at [PickNik Consulting](http://picknik.ai/)

TODO(mlautman): fix Travis badge:
[![Build Status](https://travis-ci.com/PickNikRobotics/memory_leak.svg?token=o9hPQnr2kShM9ckDs6J8&branch=master)](https://travis-ci.com/PickNikRobotics/memory_leak)

## Install

### Ubuntu Debian

> Note: this package has not been released yet

    sudo apt-get install ros-${ROS_DISTRO}-memory_leak

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

        catkin config --extend /opt/ros/${ROS_DISTRO} --cmake-args -DCMAKE_BUILD_TYPE=Release
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

Run class_name_main
```
roslaunch memory_leak class_name_main.launch
```

### Run with Debuging

Run class_name_main with GDB
```
roslaunch memory_leak class_name_main.launch debug:=true
```

Run class_name_main with Callgrind
```
roslaunch memory_leak class_name_main.launch callgrind:=true
```

Run class_name_main with Valgrind
```
roslaunch memory_leak class_name_main.launch valgrind:=true
```

## Run Inside Docker

### Prerequisite

You must have a private rsa key `~/.ssh/id_rsa` that is not password protected and is attached to your Github/Bitbucket/Gerrit accounts.
You must also have a working installation of `docker`.

1. Navigate to `$CATKIN_WS/src/memory_leak/.docker`. You should see the `Dockerfile` recipe in the directory.

1. Build the docker image

        cd $CATKIN_WS/src/memory_leak/.docker
        cp ~/.ssh/id_rsa id_rsa && docker build -t memory_leak:melodic-source .; rm id_rsa

1. Run the docker image

    * Without the gui

            docker run -it --rm memory_leak:melodic-source /bin/bash

    * With the gui (tested with Ubuntu native and a Ubuntu VM)

            . ./gui-docker -it --rm memory_leak:melodic-source /bin/bash

## Code API

> Note: this package has not been released yet

See [the Doxygen documentation](http://docs.ros.org/melodic/api/memory_leak/html/anotated.html)

## Testing and Linting

To run [roslint](http://wiki.ros.org/roslint), use the following command with [catkin-tools](https://catkin-tools.readthedocs.org/).

    roscd memory_leak
    catkin build --no-status --no-deps --this --make-args roslint

To run [catkin lint](https://pypi.python.org/pypi/catkin_lint), use the following command with [catkin-tools](https://catkin-tools.readthedocs.org/).

    catkin lint -W2 --rosdistro ${ROS_DISTRO}

Use the following command with [catkin-tools](https://catkin-tools.readthedocs.org/) to run tests.

    catkin run_tests

To run tests for just one package:

    catkin run_tests --make-args tests -- memory_leak

To view test results:

    cd $CATKIN_WS
    catkin_test_results --all

## Using ccache

ccache is a useful tool to speed up compilation times with GCC or any other sufficiently similar compiler.

To install ccache on Linux:

    sudo apt-get install ccache

To use ccache add it to your ``PATH`` in front of your regular compiler. It is recommended that you add this line to your `.bashrc`:

    export PATH=/usr/lib/ccache:$PATH