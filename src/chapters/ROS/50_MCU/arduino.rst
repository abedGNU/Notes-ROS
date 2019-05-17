*******************
Arduino
*******************

Installation
================
Installing the rosserial metapackage: ::

  sudo apt-get install ros-kinetic-rosserial

Install the rosserial-arduino client package: ::

  sudo apt-get install ros-kinetic-rosserial-arduino

Clone ::

  git clone https://github.com/ros-drivers/rosserial.git


Download and install Arduino_ IDE.

To use the serial port without root permissions: ::

	ls -l /dev/ttyACM*

or ::

	ls -l /dev/ttyUSB*
	//
	sudo usermod -a -G dialout <username>

In Arduino IDE set the Sketchbook location to ``/home/robot/arduino``. Arduino should create a folder called libraries inside it.

Open the terminal and navigate to ::

	cd /home/robot/arduino/libraries

Don't forget the dot ``.`` at the end of the following commands, it indicate current directory ::

	rosrun rosserial_arduino make_libraries.py .

After these steps, you should find the ``ros_lib`` voice in the examples of Arduino IDE.

Test the installation
----------------------

Open the example ``string_test`` from Arduino_ IDE:

.. literalinclude:: code/string_test.ino
  :name: lststring_test

Upload the sketch to the arduino board.

Open 3 different terminals and launch: ::

  roscore

  rosrun rosserial_python serial_node.py /dev/ttyACM0

  rostopic echo chatter

:download:`Download string_test.ino <code/string_test.ino>`

.. _Arduino: https://www.arduino.cc
