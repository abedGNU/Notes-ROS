
*******************
ROS basics
*******************

Overview
========

ROS basics
==========

In this section we see the basic concepts in order to create a simple package containing two nodes.
Nodes are programs, a package is a collection of programs. ROS is based on nodes, that are programs that communicate with each others, send messages via topics. A topic is the bus where a message is sent.

The master, that can be run by ```rosrun```, provides naming and registration services to the rest of nodes in the ROS system. When a node publish an information, using a message structure, it publish it on a topic. A node must subscribe to a topic in order to read that information. The master track publishers and subscribers.

Messages are data types or data structures, one can use ROS messages, std\_msgs, or create new one. User defined messages are stored in files with .msg extensions in the msg folder in the src folder of the workspace.

Topics are only one way buses, it mean if a node publish a topic, it doesn't wait an answer. If a node need to receive a reply from another node, services should be used.

Node source files are stored in src, user messages are stored in msg folder, services are stored in srv folder.

Tutorial examples can be find in /opt/ros/kinetic/share/rsopy\_tutorial or roscpp\_tutorial.

On the website of ROS, in the section wiki you can find clear and well explained tutorials about the basics.

The following two tutorial can be found on https://github.com/ros/catkin_tutorials.git

Installation
=============

.. code-block:: bash

  sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'

  // key for kinectic
  sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116
  sudo apt-get update
  sudo apt-get install ros-kinetic-desktop-full
  sudo rosdep init
  rosdep update

It's convenient if the ROS environment variables are automatically added to your bash session every time a new shell is launched:

.. code-block:: bash

  echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc
  source ~/.bashrc


If you have more than one ROS distribution installed, ~/.bashrc must only source the setup.bash for the version you are currently using.

.. code-block:: bash
  :caption: If you just want to change the environment of your current shell instead of the above you can type

    source /opt/ros/kinetic/setup.bash


.. code-block:: bash

  sudo apt-get install python-rosinstall python-rosinstall-generator python-wstool build-essential
