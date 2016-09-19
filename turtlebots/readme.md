# Turtlebots

The Turtlebot is a medium-sized mobile robot. It is based on a vacuum cleaner base and is fitted with three wooden platforms that makes it possible to mount extra equipment onto the robot. The Turtlebot is also fitted with a depth camera that lets it perceive the world around it. The brains of the robot is a small netbook.

<img src="http://wiki.ros.org/Robots/TurtleBot?action=AttachFile&do=get&target=turtlebot2_front.jpg" alt="Turtlebot 2" width= "200px"/>

## The Base
The robot is built on top of an iClebo Kobuki vacuum robot base. This features (as most robot vacuum cleaners) a differential drive system that lets is turn on the spot and drive forwards and back, much like a tank.

The base has bumper sensors in the front that will report when the robot bumps into things. The bumper can report a bump from the front, right or left. Apart from the bumper, the robot also features cliff sensors, to see whether it is driving over an edge, and drop sensors, that sense whether the wheels have dropped (when the robot is being lifted).

## The Camera
The Turtlebot is fitted with an Asus Xtion Pro Live depth camera. The difference between a depth camera and a regular RGB camera, is that the value of the pixels in the depth camera does not denote a color but a distance. The Xtion also has a regular RGB camera if that is needed.

## The Brains
The robot has an onboard computer, a small Asus netbook. Usually, robots has small embedded computers without keayboards and screens. But it is very handy to have a regular computer for prototyping. The robot is intended to run [ROS](http://www.ros.org) and there is a whole host of information and guides for using ROS and Turtlebots on the [official ROS wiki](http://wiki.ros.org/Robots/TurtleBot) and on the [official Turtlebot homepage from OSRF](http://learn.turtlebot.com), also Clearpath Robotics has a [set of guides](https://www.clearpathrobotics.com/assets/guides/turtlebot/index.html).

## Wi-Fi Router
It is convenient to be able to connect to the Turtlebot wirelessly, but the University networks are too limiting, so every Turtlebot set includes a wireless router. See [how to setup the router](turtlebots/setup-the-router.md).
