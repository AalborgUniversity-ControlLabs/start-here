# Working With the Crust Crawler Arms

Here we will look at how to work with the actual Crust Crawler arms, though the use of the ArbotiX-M. The arms have five degrees of freedom, with each degree controlled by a Dynamixel servo. We will go through:

1. How the communication with the servos work.
2. How to read the position, velocity, acceleration and torque of the servos.
3. How to give the servos a setpoint.
4. How to set the controller's PID parameters.
5. What NOT to do.

This tutorial assumes that you have a working installation of the Arduino IDE, and connection to the ArbotiX-M as described in [the previous tutorial](./getting-started-with-arbotix-m.md).

## How the Communication With the Servos Work
The Dynamixel servos are called smart servos. This is because they have both a motor, reduction gear, encoder and a position controller; traditional servos rely on an external controller. This enables us to simply control the servo by giving it a position setpoint, but if we choose, we can control the torque of the motor directly.

The communication works by polling (asking) the servo for a piece of information, which it then provides. The protocol is described on [Robotis' support page](http://support.robotis.com/en/techsupport_eng.htm#product/dynamixel/dxl_communication.htm). Basically, the servos has a memory-mapped interface called the control table with a range of registers holding static and dynamic content. For example, have a look at the MX-64 servo's [control table at the Robotis support page](http://support.robotis.com/en/techsupport_eng.htm#product/dynamixel/mx_series/mx-64.htm#Control_Table).

Every register can be read, but only some can be written to (and some can be written to, but shouldn't be). E.g. a couple of interesting registers to look at is registers 36 and 37, which hold the present position of the servo. The reason for having two registers is that each register holds 8 bits of information, but the encoder on the servo provides 10 bits of resolution. So register 36 holds the lowest eight bits and 37 holds the highest two bits. To read the two registers, the ArbotiX-M must send a request to the relevant servo asking for two bytes starting from register 36, and the servo will respond with a packet containing these. This is all outlined in the article on the protocol from Robotis.

Fortunately, a set of convenience functions for the ArbotiX-M has been written, so that you do not have to fiddle with high bytes and low bytes, checksums and sending and receiving raw bytes to and from the serial port. The basic convenience funtions are:
```
// From ax12.h
int ax12GetRegister(int id, int regstart, int length);
void ax12SetRegister(int id, int regstart, int data);
void ax12SetRegister2(int id, int regstart, int data);
```
These functions are defined in `ax12.h`, a library originally written for the ArbotiX to control AX-12 servos, but now also works with most other Dynamixel servos.

With these three functions we can read from or (attempt to) write to any register in the servos. Note that the functions needs and ID. This is name of the sevo that we want to communicat to. The servos are numbered from 1 to 5:

 Servo        | ID
 -------------|---
 Base         | 1
 Shoulder     | 2
 Elbow        | 3
 Left finger  | 4
 Right finger | 5

For even more comfort a set of macros is also defined in `ax12.h`:
```
// From ax12.h
#define SetPosition(id, pos) (ax12SetRegister2(id, AX_GOAL_POSITION_L, pos))
#define GetPosition(id) (ax12GetRegister(id, AX_PRESENT_POSITION_L, 2))
#define TorqueOn(id) (ax12SetRegister(id, AX_TORQUE_ENABLE, 1))
#define Relax(id) (ax12SetRegister(id, AX_TORQUE_ENABLE, 0))
```
Using these, this code:
```
int pos = ax12GetRegister(1, 36, 2);
```
can be written as:
```
int pos = GetPosition(1);
```
which is easier to read.

Equivalently we can send a setpoint:
```
ax12SetRegister2(1, 36, 2500);
```
Here we are using `ax12SetRegister2`, as we need to set two registers at once. Alternatively, we can use the convenience-convenice function:
```
SetPosition(1, 2500);
```
