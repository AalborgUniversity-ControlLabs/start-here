# Crust Crawler Arms
We have four robotic arms in the lab. They have five degrees of freedom and are controlled by an Arduino-compatible controller (or alternatively directly from a PC).

The arms are based on the Pro-series robotic arm kits from [Crust Crawler Robotics](http://www.crustcrawler.com/products/ProRoboticArm/). The controller boards are the Arbotix-M from [Trossen Robotics](http://www.trossenrobotics.com/p/arbotix-robot-controller.aspx).

##### Tutorial
Here is a [tutorial on getting started with the Arbotix-M](./getting-started-with-arbotix-m.md).

#### Dynamixel Servos
The arms are actuated by five Dynamixel servos from Robotis. An MX-64 in the base, an MX-106 in the shoulder, an MX-64 in the elbow and two MX-28 in the hand. The Dynamixel servos incorporates both a DC motor, reduction gear, a magnetic aboslute encoder and an ARM-based PID controller. They can be controlled via position setpoints or alternatively acceleration or torque setpoints. Robotis provides information on the servos on their [support page](http://support.robotis.com/en/).

#### The Arbotix-M Controller
The original Arbotix controller was developed by Michael Ferguson while at [Vanadium Labs](http://www.vanadiumlabs.com/) (if you like robotics, you are bound to see this guys name). The Arbotix was ment to be a low-cost controller for small robot projects, that was easy to program. Trossen Robotics started producing and selling the Arbotix controller and refined it into the Arbotix-M version.

The controller is Arduino-compatible. This means that with little effort, the Arduino IDE can be used to program the controller. The Arbotix-M is based on an Atmel ATmega644PA-AU, which basically has one more serial port than the ATmega328P. This extra port is being used to communicate with the Dynamixel servos, while the first serial port can beused for communicationg with a PC, as we are used to from the standard Arduino Uno.

#### RS-485 Connection
The servos are connected to the controller through a differential RS-485 connection. Originally the Arbotix-M was designed to use a TTL serial connection, but we decided on the more robust RS-485. Thus, the TTL input/output from the Arbotix-M is routed though a RS-485 level changer.
