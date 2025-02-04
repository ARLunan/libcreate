## Modified to work on Create 1 base with analog gyroscope or its emulation ##

Create 1 firmware has a known bug described here: https://github.com/AutonomyLab/create_robot/issues/28

Traditionally old Turtlebots relied on analog 1-axis gyroscope (ENC-03R, LPR5150AL) connected to pin 4 (analog input) of its Cargo Bay DB25 connector.

A better option is using an MPU9250 "emulation" with Arduino Nano, providing the same analog output (see [MPU9250GyroTurtlebot](https://github.com/slgrobotics/Misc/tree/master/Arduino/Sketchbook/MPU9250GyroTurtlebot))

This configuration works under _ROS2 Jazzy_ with _Create 1 driver_ here: https://github.com/slgrobotics/create_robot

Refer to detailed setup instructions here: https://github.com/slgrobotics/robots_bringup/tree/main/Docs/Create1

Odometry calculations: see *~/robot_ws/src/libcreate/src/create.cpp : 135*

# libcreate #

C++ library for interfacing with iRobot's Create 1 and 2 as well as most models of Roomba. [create_robot](http://wiki.ros.org/create_robot) is a [ROS](http://www.ros.org/) wrapper for this library.

* [Code API](http://docs.ros.org/noetic/api/libcreate/html/index.html)
* Protocol documentation:
  - [`V_1`](https://drive.google.com/file/d/0B9O4b91VYXMdUHlqNklDU09NU0k/view?usp=sharing&resourcekey=0-KxMpRPBMsGAj7eSYC_9ewA) (Roomba 400 series )
  - [`V_2`](https://drive.google.com/file/d/0B9O4b91VYXMdMmFPMVNDUEZ6d0U/view?usp=sharing&resourcekey=0-bqKH8xhtWdYtTik_LLWo9Q) (Create 1, Roomba 500 series)
  - [`V_3`](https://drive.google.com/file/d/0B9O4b91VYXMdSVk4amw1N09mQ3c/view?usp=sharing&resourcekey=0-rKvug2IzC7nj4zV31EJtww) (Create 2, Roomba 600-800 series)
* Author: [Jacob Perron](http://jacobperron.ca) ([Autonomy Lab](http://autonomylab.org), [Simon Fraser University](http://www.sfu.ca))
* Contributors: [Mani Monajjemi](http:mani.im), [Ben Wolsieffer](https://github.com/lopsided98), [Josh Gadeken](https://github.com/process1183)

## Build Status ##

![Build Status](https://github.com/AutonomyLab/libcreate/workflows/Build%20and%20test/badge.svg)

## Dependencies ##

* [Boost System Library](http://www.boost.org/doc/libs/1_59_0/libs/system/doc/index.html)
* [Boost Thread Library](http://www.boost.org/doc/libs/1_59_0/doc/html/thread.html)
* [Optional] [googletest](https://github.com/google/googletest)

### Install ###

        sudo apt-get install build-essential cmake libboost-system-dev libboost-thread-dev

        # Optionally, install gtest for building unit tests
        sudo apt-get install libgtest-dev
        cd /usr/src/gtest
        sudo cmake CMakeLists.txt
        sudo make
        sudo cp *.a /usr/lib

#### Serial Permissions ####

User permission is requried to connect to Create over serial. You can add your user to the dialout group to get permission:

        sudo usermod -a -G dialout $USER

Logout and login again for this to take effect.

## Build ##

Note, the examples found in the "examples" directory are built with the library.

#### cmake ####

        git clone https://github.com/AutonomyLab/libcreate.git
        cd libcreate
        mkdir build && cd build
        cmake ..
        make -j

#### catkin ####

Requires [catkin_tools](https://catkin-tools.readthedocs.io/en/latest/).

        mkdir -p create_ws/src
        cd create_ws
        catkin init
        cd src
        git clone https://github.com/AutonomyLab/libcreate.git
        catkin build

## Running Tests ##

To run unit tests, execute the following in the build directory:

        make test

## Known Issues ##

* _Clock_ and _Schedule_ buttons are not functional. This is a known bug related to the firmware.
* Inaccurate odometry angle for Create 1 ([#22](https://github.com/AutonomyLab/libcreate/issues/22))
* Some 600 series models incorrectly report the OI Mode in their sensor stream ([create_robot #64](https://github.com/AutonomyLab/create_robot/issues/64))
  - To enable or disable the OI Mode reporting workaround, pass `true` or `false` to `setModeReportWorkaround()`
