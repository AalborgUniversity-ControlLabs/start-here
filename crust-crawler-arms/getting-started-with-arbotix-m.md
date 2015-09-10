# Getting Started with the Arbotix-M
The Arbotix-M is an open-source robot controller, it is Arduino-compatible and fairly easy to program. It was developed by Michael Ferguson at Vanadium Labs and refined by Trossen Robotics.

Trossen Robotics has a lot of documentation on their [webpage](http://www.trossenrobotics.com/p/arbotix-robot-controller.aspx) under the 'Documentation and Downloads' tab. Therefore the documentation here is a bit sparingly, and may focus on things specific to the setup here in the Automation and Control Labs.

##### These are the steps to setup your PC to program the Arbotix-M and run a demo. Each step is explained below.
1. Download the Arduino IDE (version 1.0.6)
2. Download the Arbotix-M libraries and hardware description files.
3. Connect to the ArbotiX-M and upload the sketch.
4. Watch it work (or start debugging.)

## Download the Arduino IDE
You need to install the Arduino IDE to program the controller. Currently (10-09-2015), the Arbotix-M runs on the old Arduino 1.0.6 version. This means that you will need to get and install the old IDE. This, however is actually quite simple. If you have the newest IDE installed already, you don't need uninstall it, the two IDEs can live along side each other. The files are [here](https://www.arduino.cc/en/Main/OldSoftwareReleases).

##### Windows
There are two Windows installers, it is actually easiest to get the non-admin installer. This way, the software will no be installed into C:\program files\ but you can choose to put it wherever. This is clever in that it doen't mess with a previously installed Arduino IDE, and you can customize the IDE for use with the Arbotix-M and Crust Crawler Arms, and when you are done using this old IDE, you can simply delete the install folder, no Windows install/uninstall stuff.

##### Linux
If you are on Linux, the same approach applies; just get the appropriate tarball (32 or 64 bit) and unpack it wherever you like. For example:
```
tar xzvf arduino-1.0.6-linux64.tgz
```

#### Test Your Installation
Run the executable, `arduino.exe`, in the newly created `arduino-1.0.6` folder; and the IDE should be starting up. If you have another (newer) Arduino IDE installed, check `Help -> About Arduino` to see that version 1.0.6 is displayed.

> ##### Why use the old Arduino IDE?
The Arduino IDE 1.0.6 has a different implementation of the Arduino core code than the 1.6. Actually looking at the [release notes](https://www.arduino.cc/en/Main/ReleaseNotes), the Arduino IDE 1.0.1 went into a 1.5 BETA branch in late 2012. The new IDE was an attemt to make a unified IDE for both 8-bit AVR and 32-bit ARM based Arduinos. While the 1.5 BETA developed into 1.6 and went out of the BETA stage, the 1.0.1 developed into the 1.0.6. The big difference is how the hardware definition files are handled in the IDE. Also the original Arduino Serial code is too slow to handle the 1Mbps that we run the Crust Crawler Arms at, so a custom implementation was developed in Vanadium Labs. But looking at the developments in the [code on GitHub](https://github.com/vanadiumlabs/arbotix) it seems branches for the 1.5 and 1.6 IDE are popping up. So maybe we can use the new IDE in the near future.

## Download the Arbotix-M Libraries and Hardware Description Files
The Arbotix-M is arduino _compatible_, not a true arduino. So we need to install some additional hardware description files and finally some libraries to work with the Dynamixel servos. You can download a [zip-file](https://github.com/trossenrobotics/arbotix/archive/master.zip) from Trossen Robotics with everything you need.

In the zip-archive there are three folders, you have to manually move these into your sketchbook folder.

Now, pay attention here. The sketchbook folder is not the same as your newly created installation folder. The default folder is `~/Documents/Arduino` (~ being your user folder). This will most likely be the same for any Arduino installation on your system, so to keep things separate, change the sketchbook folder location in the IDE to a folder exclusively for use with this project, e.g. `~/Documents/Arduino-arbotix`. You do this in `Files -> Preferences` and simply writing in your desired location in the `Sketchbook Location` box. The IDE will create the new folder and populate it with a ´libraries` folder.

Now, extract the three folders (hardware/libraries/ArbotiX Sketches) in the newly downloaded zip-archive into your sketchbook folder. You can test if this was succesful by checking `File -> Sketchbook ->` in the IDE, this should now contain a menu with ArbotiX Sketches. If the menu does not show up, you may need to reopen your Arduino IDE.

#### Test the Installation
In this new menu, there is a set of test sketches; load ArbotixBlink. Also, set the board to the Arbotix-M by going to `Tools -> Board` and choose `Arbotix w/ RX Shield`. Now, click the tick mark in the upper left corner of the IDE to verify that the sketch can compile.

> ##### Why Arbotix w/ RX Shield
We use RS-485 to communicate with the Dynamixel servos on the Crust Crawler arms. The plain ArbotiX-M does only support TTL communications, so we have added a RS-485 driver to the output. Originally the ArbotiX (without the -M) had an option to mount a shield PCB that provided the RS-485 conversion; this was called an RX-shield as it was needed to communicate with the RX-series Dynamixel servos. We use the new MX-series, which exists in both TTL and RS-485 configurations, but we use the RS-485 version. However, choosing the RX-shield option tells the ArbotiX-M that it is driving a RS-485 level-changer.

## Connect to the ArbotiX-M and Upload the Sketch
Connecting to the ArbotiX-M is a matter of plugging in a USB cable to your PC and the UARTSBee programmer mounted on the Crust Crawler arm. In their guide, Trossen Robotics goes on to tell you to install FTDI drivers for the UARTSBee, however, most up to date operating systems seem to have these drivers already. The programmer should be connected to the ArbotiX-M already, if not, refer to the image below.

![Connect the PC, UARTSBee and ArbotiX-M](http://learn.trossenrobotics.com/cache/multithumb_thumbs/b_450_0_16777215_00__images_tutorials_arbotixM_arbotixm_uartsbee.png)

##### Trouble with the UARTSBee and XBee Connections
To complicate things, the programmer and the wireless module, XBee, uses the same physical serial port, and can as such not be connected at the same time. Trossen Robotics tells you to unmount the XBee before programming, this would, however, wear out the socket and make for a tedious programming experience. So we have modified the board with a switch, so that you can choose which device is connected to the serial port. No need to mount and unmount XBees and UARTSBees.

Now you have plugged in the USB cable, and a new COM port has appeared. On Windows, check your device manager under ports for the COM number. On Linux, it is most likely located under `/dev/ttyUSB0` if no other USB based serial port is plugged in. Go to your IDE and choose the coresponding serial port under `Tools -> Serial Port`.

You are now ready to upload the ArbotixBlink test sketch. Click the upload button in the upper left corner of the IDE; the lights on the programmer will be flashing as it is working. After a short while, the user LED on the ArbotiX-M will be blinking on and off with a one second interval. Success! You set up your PC and programmed the AbotiX-M!

## Questions
#### Why Use a Microcontroller?
It is convenient to use the ArbotiX-M for controlling robots, as there is no operating system on it. This means that there is nothing else running on the controller but what you upload to it. Thus, you can achieve hard real-time guarantees. This is important when doing e.g. trajectory control, where setpoints must be sent to the robot at exact points in time. If you were to try the same on your PC, then suddenly, the trajectory controller might be interrupted because your operating systems wants to install system updates, synchronize your dropbox folder or something else. Actually, your PC handles hundreds or thousands of interrupts every second, where the microcontroller handles none, unless you tell it to.
