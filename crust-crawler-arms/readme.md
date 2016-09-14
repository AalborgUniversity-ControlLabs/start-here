# Crust Crawler Arms
We have a class set of robotic arms in the lab. They have five degrees of freedom and are controlled by an Arduino-compatible controller (or alternatively directly from a PC).

The arms are based on the Pro-series robotic arm kits from [Crust Crawler Robotics](http://www.crustcrawler.com/products/ProRoboticArm/). The controller boards are the Arbotix-M from [Trossen Robotics](http://www.trossenrobotics.com/p/arbotix-robot-controller.aspx).

##### Tutorial
Here is a [tutorial on getting started with the Arbotix-M](./getting-started-with-arbotix-m.md).

#### Dynamixel Servos
The arms are actuated by five Dynamixel servos from Robotis. An MX-64 in the base, an MX-106 in the shoulder, an MX-64 in the elbow and two MX-28 in the hand. The Dynamixel servos incorporates both a DC motor, reduction gear, a magnetic aboslute encoder and an ARM-based PID controller. They can be controlled via position setpoints or alternatively acceleration or torque setpoints. Robotis provides information on the servos on their [support page](http://support.robotis.com/en/).

#### The Arbotix-M Controller
The original Arbotix controller was developed by Michael Ferguson while at [Vanadium Labs](http://www.vanadiumlabs.com/) (if you like robotics, you are bound to see this guy's name). The Arbotix was meant to be a low-cost controller for small robot projects, that was easy to program. Trossen Robotics started producing and selling the Arbotix controller and refined it into the Arbotix-M version.

The controller is Arduino-compatible. This means that with little effort, the Arduino IDE can be used to program the controller. The Arbotix-M is based on an Atmel ATmega644PA-AU, which basically has one more serial port than the ATmega328P. This extra port is being used to communicate with the Dynamixel servos, while the first serial port can beused for communicationg with a PC, as we are used to from the standard Arduino Uno.

> ##### Why Use a Microcontroller?
It is convenient to use the ArbotiX-M for controlling robots, as there is no operating system on it. This means that there is nothing else running on the controller but what you upload to it. Thus, you can achieve hard real-time guarantees. This is important when doing e.g. trajectory control, where setpoints must be sent to the robot at exact points in time. If you were to try the same on your PC, then suddenly, the trajectory controller might be interrupted because your operating systems wants to install system updates, synchronize your dropbox folder or something else. Actually, your PC handles hundreds or thousands of interrupts every second, where the microcontroller handles none, unless you tell it to.

#### RS-485 Connection
The servos are connected to the controller through a differential RS-485 connection. Originally the Arbotix-M was designed to use a TTL serial connection, but we decided on the more robust RS-485. Thus, the TTL input/output from the Arbotix-M is routed though a [RS-485 level changer](https://www.sparkfun.com/products/10124).

#### Power Hub Board
The four-wire Dynamixel connectors (which are actually [Molex SPOX 50-37-5043](http://www.molex.com/molex/products/datasheet.jsp?part=active/0050375043_CRIMP_HOUSINGS.xml&channel=Products)) carry the differential RS-485 signal on two of the wires and the 12 V supply and ground on the two other ([reference](http://support.robotis.com/en/product/dynamixel/dxl_mx_main.htm)). The 12 V are supplied through the [power hub board](http://www.trossenrobotics.com/6-port-rx-power-hub).
The power could be applied though the Arbotix-M, but, this way, the Abotix-M can be disconnected while still having power to the servos so another RS-485 controller can be used instead, e.g. a USB-to-RS-485 from a PC.

#### USB to RS-485 Adaptors
We have a handful of USB to RS-485 converter cables with Molex connectors. You can use these to connect your laptop directly to the Crust Crawler arms. This way you can control them, or you can sniff the raw data being sent to the servos. You can use the [Python module](https://github.com/AalborgUniversity-ControlLabs/crust-crawler-arms-python) developed here at Aalborg University to control and read from the servos. It provides some low-level commands to send raw data and some high-level commands for easier readability and also some diagnostics tools for when things go wrong.
