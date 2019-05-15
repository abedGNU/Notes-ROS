
*******************
ROS worksapce
*******************

Catkin workspace
================

Like Arduino, Eclispe and other IDE, to develop in ROS you need to create a workspace.
A workspace mainly contain the source code folder and the compiled code and eventually other folders and files.

In order to create a workspace in ROS, ``catkin`` tools are used. These tools automate some job in Cmake.
There are 3 ways to use catkin. The first one is the most used. The second one ``catkin_tools`` is under development. The third one ``catkin_simple`` can be used in order to simplify the CMakeLists.txt file.

Before using the catkin commands, you need to create a workspace directory and a ``src`` subdirectory. You can call the workspace as you want. The directories can be created using the ``terminal`` or using the desktop utilities. Usually we will use the terminal. If you want to use an IDE, ``RoboWare Studio`` and ``Qt`` plugin ``ros_qtc_plugin`` are good choices.

catkin
------

We will create a workspace called ``catkin_ws`` and a subdirectory ``src`` in the home directory of linux. The following code will create a folder called ``src``. The ``-p`` option will create the folder ``catkin_ws`` if it doesn't exist ::

  mkdir -p ~/catkin_ws/src

Navigate to the ``src`` folder and initialize the workspace. The command ``catkin_init_workspace`` should be executed within the folder ``src`` ::

  cd ~/catkin_ws/src
  catkin_init_workspace

Go back to the workspace directory and type ```catkin_make``` in order to build the source code ::

  cd ~/catkin_ws/
  catkin_make

In order to build the worksapce after any modification of the source code, the ``catkin_make`` command should be run from the workspace directory.

catkin tools
------------

This is a python package that can be used to create and build a ROS workspace. It is not installed by default. Run the following commnad to install it ::

  sudo apt-get install python-catkin-tools

The documentation of this package can be found:

  https://github.com/catkin/catkin_tools

  http://catkin-tools.readthedocs.org/

Once the workspace and src folders are created, to initilize the workspace run the command ``catkin init`` from the workspace directory not from the ``src`` ::

  cd ~/catkin_ws/
  catkin init

To build the worksapce run ::

  catkin build

The command ``catkin build`` will initilize the workspace if it was not initilized by the ``catkin init`` command.
The ``catkin build`` command can be run from any subdirectory of the workspace.

catkin simple
-------------

Wokspace subdirectory
---------------------

Once is build the workspace will contain the ``src``, ``build`` and ``devel`` folders. These folders are create by all the prvious methods.

Environment variable
--------------------

The catkin_make command is a convenience tool for working with catkin workspaces. Running it the first time in your workspace, it will create a CMakeLists.txt link in your 'src' folder. Additionally, if you look in your current directory you should now have a ``build`` and 'devel' folder. Inside the 'devel' folder you can see that there are now several setup.\*sh files. Sourcing any of these files will overlay this workspace on top of your environment. To understand more about this see the general catkin documentation: catkin. Before continuing source your new setup.\*sh file:

.. code-block:: bash

  // this will be valid only for the opened terminal
  	source devel/setup.bash

To make it permanent do the following:

.. code-block:: bash

	echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
	source ~/.bashrc

To make sure your workspace is properly overlayed by the setup script, make sure ROS\_PACKAGE\_PATH environment variable includes the directory you're in.

.. code-block:: bash

  echo $ROS_PACKAGE_PATH

Creating a ROS Package
======================

Catkin
-------------

.. code-block:: bash

  // This is an example, do not try to run this
  // catkin_create_pkg <package_name> [depend1] [depend2] [depend3]

.. code-block:: bash

	cd ~/catkin_ws/src

	// create a package
	catkin_create_pkg beginner_tutorials std_msgs rospy roscpp

	// build all packages in the workspace
	cd ~/catkin_ws
	catkin_make

	//To add the workspace to your ROS environment you need to source the generated setup file
	. ~/catkin_ws/devel/setup.bash

catkin_tools
-------------

catkin_simple
-------------
