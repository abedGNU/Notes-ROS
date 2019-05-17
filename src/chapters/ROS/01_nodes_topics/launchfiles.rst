*******************
Launch_ files
*******************

Basic launch file
========================

Ros launch ``talker_listener.launch`` file that execute 2 nodes:

.. code-block:: xml
  :caption: talker_listener.launch

  <launch>
    <node name="listener" pkg="roscpp_tutorials" type="listener_node" output="screen"/>
    <node name="talker" pkg="roscpp_tutorials" type="talker_node" output="screen"/>
  </launch>

The previous launch file execute one instance of the ``listener_node`` with a the name ``listener``, and one instance of the executable ``talker_node`` with the name ``talker``, that are part of the package ``roscpp_tutorials``.
So the ``type`` id the name of the compiled node. The ``name`` is the name of the instance of the node (process).
The tag ``output`` have value ``screen``, it tell ROS to show the outputs of the nodes in the terminal.
If ``roscore`` is not running, ``roslaunch`` execute it.

Open a terminal and run the roslaunch_ command::

  roslaunch roscpp_tutorials talker_listener.launch

Of course the package ``roscpp_tutorials`` should be in your active workspace or installed on your computer. Or you substitute the package name and node names with your own.

Clone the repository in your workspace ::

  git clone https://github.com/ros/ros_tutorials.git

Or install it ::

  sudo apt-get install ros-kinetic-roscpp-tutorials

Parameters
============

You can also set parameters on the Parameter Server. These parameters will be stored on the Parameter Server before any nodes are launched.

.. code-block:: xml

    <launch>
      <param name="somestring1" value="bar" />
      <!-- force to string instead of integer -->
      <param name="somestring2" value="10" type="str" />

      <param name="someinteger1" value="1" type="int" />
      <param name="someinteger2" value="2" />

      <param name="somefloat1" value="3.14159" type="double" />
      <param name="somefloat2" value="3.0" />

      <!-- you can set parameters in child namespaces -->
      <param name="wg/childparam" value="a child namespace parameter" />

      <!-- upload the contents of a file to the server -->
      <param name="configfile" textfile="$(find roslaunch)/example.xml" />
      <!-- upload the contents of a file as base64 binary to the server -->
      <param name="binaryfile" binfile="$(find roslaunch)/example.xml" />
    </launch>

Substitution args
====================
.. code-block:: xml
  :caption: launch_file.launch

   <arg name="gui" default="true" />
   <param name="foo" value="$(arg my_foo)" />

   <node name="add_two_ints_server" pkg="beginner_tutorials" type="add_two_ints_server" />
   <node name="add_two_ints_client" pkg="beginner_tutorials" type="add_two_ints_client" args="$(arg a) $(arg b)" />

Execution example: ::

  roslaunch beginner_tutorials launch_file.launch a:=1 b:=5

Namespaces
=============

Including files
=================


.. _roslaunch: http://wiki.ros.org/roslaunch
.. _Launch: http://wiki.ros.org/roslaunch
