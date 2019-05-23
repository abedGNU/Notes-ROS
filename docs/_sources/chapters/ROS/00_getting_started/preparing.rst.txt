
*******************
Preparing
*******************

Installation
=============
Refer to the official website for more information. The instructions below are valid for almost all releases. In this tutorial ``kinetic`` and ``ubuntu`` 16.04 are used.

Open a terminal: ::

  sudo apt-get install python-rosinstall python-rosinstall-generator python-wstool build-essential

  sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'

  // key for kinectic
  sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116
  sudo apt-get update
  sudo apt-get install ros-kinetic-desktop-full
  sudo rosdep init
  rosdep update

After installation you need to tell ubunut where ROS is installed ::

  echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc
  source ~/.bashrc

Or you open the hidden file ``.bashrc`` with a text editor and add ::

    source /opt/ros/kinetic/setup.bash

To check the installation in a terminal type: ::

  roscore

Catkin workspace
================

Like Arduino, Eclispe and other IDE, to develop in ROS you need to create a workspace.
A workspace mainly contain the source code folder and the compiled code and eventually other folders and files.

In order to create a workspace in ROS, ``catkin`` tools are used. These tools automate some job in ``Cmake``.
There are 3 ways to use catkin. The first one is the most used. The second one ``catkin_tools`` is under development.
The third one ``catkin_simple`` can be used in order to simplify the ``CMakeLists.txt`` file.

Before using the catkin commands, you need to create a workspace directory and a ``src`` subdirectory.
You can call the workspace as you want. The directories can be created using the ``terminal`` or using the desktop utilities.
Usually we will use the terminal.

If you want to use an IDE, ``RoboWare Studio`` and ``Qt`` plugin ``ros_qtc_plugin`` are good choices.

catkin
------

We will create a workspace called ``catkin_ws`` and a subdirectory ``src`` in the home directory of linux.
The following code will create a folder called ``src``. The ``-p`` option will create the folder ``catkin_ws`` if it doesn't exist ::

  mkdir -p ~/catkin_ws/src

Navigate to the ``src`` folder and initialize the workspace.
The command ``catkin_init_workspace`` should be executed within the folder ``src`` ::

  cd ~/catkin_ws/src
  catkin_init_workspace

Go back to the workspace directory and type ```catkin_make``` in order to build the source code ::

  cd ~/catkin_ws/
  catkin_make

In order to build the worksapce after any modification of the source code, the ``catkin_make`` command should be run from the workspace directory.

catkin tools
------------

This is a python package that can be used to create and build a ROS workspace.
It is not installed by default. Run the following commnad to install it ::

  sudo apt-get install python-catkin-tools

The documentation of this package can be found:

  https://github.com/catkin/catkin_tools

  http://catkin-tools.readthedocs.org/

Once the workspace and src folders are created, to initialize the workspace run the command ``catkin init`` from the workspace directory not from the ``src``::

  cd ~/catkin_ws/
  catkin init

To build the workspace run ::

  catkin build

The command ``catkin build`` will initialize the workspace if it was not initialized by the ``catkin init`` command.
The ``catkin build`` command can be run from any sub-directory of the workspace, contrary to ``catkin_init_workspace``.

catkin simple
-------------

Workspace sub-directory
-------------------------

Once is build the workspace will contain the ``src``, ``build`` and ``devel`` folders. These folders are create by all the previous methods.

Environment variable
--------------------
When you create the worksapce the first time, after the compilation run the command: ::

  // this will be valid only for the opened terminal
  	source devel/setup.bash

To make it permanent do the following: ::

	echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
	source ~/.bashrc

Make sure ``ROS\_PACKAGE\_PATH`` environment variable includes the worksapce in use: ::

  echo $ROS_PACKAGE_PATH

Illustration
-----------------
Remember you can put the workspace in any directory and give any name.

??????????????? make a gif

Creating a ROS Package
======================
Simply a package is a collection of programs.

Catkin
-------------

When creating a package, a name should be given and all dependencies: ::

  catkin_create_pkg <package_name> [depend1] [depend2] [depend3]

For example we create ``beginner_tutorials`` package that depend on ``std_msgs``, ``rospy``, ``roscpp`` ::

	cd ~/catkin_ws/src

	// create a package
	catkin_create_pkg beginner_tutorials std_msgs rospy roscpp

	// build all packages in the workspace
	cd ~/catkin_ws
	catkin_make

??????????????? make a gif

catkin_tools
-------------

catkin_simple
-------------
