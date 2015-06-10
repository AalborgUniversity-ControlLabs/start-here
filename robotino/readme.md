# Robotino
The Robotino is a mobile robot very much like the Turtlebots, but with a much more sturdy design and omnidirectional wheels. It was designed by Festo.

We have a Robotino version 2, the newest version is 3. We acquired the robot in 2006 for a PhD project on robotics in human environments. You may have see pictures of Mikael Svenstrup and the Robotino with an attached black and red smiley-face. It is now used in the patient@home project, where it will form the basis of a socially interactive robot.

I was initially made to run with Player/Stage, but efforts are now made to make it run ROS, which is a natural extension of Player.

## Getting access to the Robotino
The Robotino creates a WiFi hotspot named "RobotinoAP1.110" without security. When loggin on to this, your computer will get an IP on a 172.26.x.x network. The default address of the Robotino is 172.26.1.1. Log on using SSH:

    ssh robotino@172.26.1.1

| Usernames | Passwords |
| --------- | --------- |
| robotino  | robotino  |
| root      | dorp6     |

Remember to use the root access sparingly to avoid messing stuff up.

## Making It Run ROS
ROS will not run on the PC104+ in the Robotino, it will connect to a PC running ROS through an API.

##### On the Robotino
The ROS packages use the new `API2` on the Robotino rather than the old `COM` interface. This is the interface that the ROS computer uses to communicate to the PC104+ that is the low-level controller on the Robotino.

In order to use API2, the old COM daemons needs to be disabled and new API2 daemons installed. It is quite simple to do, as REC GmbH has made nice Debian packages that does most of the work.

These instructions are taken from [the openrobotino wiki](http://wiki.openrobotino.org/index.php?title=Install_daemons_v2).

I (Karl D. Hansen) have not connected the Robotino to the internet, so instead i downloaded the relevant .debs:

    wget http://doc.openrobotino.org/download/packages/i386/rec-rpc-current.php
    wget http://doc.openrobotino.org/download/packages/i386/robotino-common_current.php
    wget http://doc.openrobotino.org/download/packages/i386/robotino_daemons_current.php
    wget http://doc.openrobotino.org/download/packages/i386/robotino-api2_current.php
    wget http://doc.openrobotino.org/download/packages/i386/robotino-examples_current.php

The PHP, automatically choses the current stable version.

Next copy the files to the Robotino wit SCP:

    scp robotino@172.26.1.1: api2/*

I downloaded the .debs to a folder named "api2", so SCP just transfered all five files. Then log into the Robotino and install the .debs:

    dpkg -i rec-rpc-qt4.5.0_*_i386.deb
    dpkg -i robotino-common_*_i386.deb
    dpkg -i --auto-deconfigure robotino-daemons_*_i386.deb
    dpkg -i robotino-api2_*_i386.deb
    dpkg -i robotino-examples_*_i386.deb

The "robotino-deamons" package will stop and uninstall the previous daemons. That is it.

##### On the ROS PC
Next, we need to install ROS and the relevant Robotino ROS packages to a PC.

Installing ROS is well treated on the [ROS wiki](http://wiki.ros.org/ROS/Installation). 

Next, create a Catkin workspace. This is also treated on the [wiki](http://wiki.ros.org/catkin/Tutorials/create_a_workspace).

Now we need to get the Robotino ROS packages. They are not on the ROS servers, we need to get them via SVN from openrobotino.org. I use wstool to manage the version control. See chapter 3 in this [workspace overlaying tutorial](http://wiki.ros.org/catkin/Tutorials/workspace_overlaying).
    
    wstool init

The code is located in http://svn.openrobotino.org/robotino-ros-pkg/trunk/catkin-pkg/ with all the packages in the same repository. I chose to make an entry in the .rosinstall file for each package. Example for `robotino_node`:

    wstool set robotino_node --svn http://svn.openrobotino.org/robotino-ros-pkg/trunk/catkin-pkg/robotino_node/

Then, get the actual code:

    wstool update

Once the code has been downloaded, we need to make sure it's dependencies are met, rosdep will help us here. However, most of the packages depend on robotino_node, which is not in rosdeps repositories and will give an error. So we append the `-r`flag to ignore errors. Beware that this may ignore other errors as well. 

    rosdep install -r --from-paths ./src/

This assumes you are located in the catkin workspace root.

When the code is downloaded and dependencies met and you are located in the workspace root, execute the build command:

    catkin_make

And stuff should start compiling.

Finally, if you are lucky, you can fire off:

    roslaunch robotino_node robotino_node.launch hostname:=172.26.1.1

and
    
    roslaunch robotino_teleop joystick_teleop.launch

To move your robot around.
