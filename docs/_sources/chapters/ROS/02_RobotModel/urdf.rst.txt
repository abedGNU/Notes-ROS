****************************************
Unified Robot Description Format URDF_
****************************************
.. note::
  Refer to URDF-Tutorials_ and clone the URDF-Tutorials-repo_ ::

    git clone https://github.com/ros/urdf_tutorial.git

Launch file
=============
Launch ::

  <launch>
    <arg name="model" default="$(find urdf_tutorial)/urdf/01-myfirst.urdf"/>
    <arg name="gui" default="true" />
    <arg name="rvizconfig" default="$(find urdf_tutorial)/rviz/urdf.rviz" />

    <param name="robot_description" command="$(find xacro)/xacro --inorder $(arg model)" />
    <param name="use_gui" value="$(arg gui)"/>

    <node name="joint_state_publisher" pkg="joint_state_publisher" type="joint_state_publisher" />
    <node name="robot_state_publisher" pkg="robot_state_publisher" type="state_publisher" />
    <node name="rviz" pkg="rviz" type="rviz" args="-d $(arg rvizconfig)" required="true" />
  </launch>

URDF basics
============

Link
------

Joint
------

Tansmission
-------------




.. _URDF: wiki.ros.org/urdf
.. _URDF-Tutorials: http://wiki.ros.org/urdf/Tutorials
.. _URDF-Repo: https://github.com/ros/urdf.git
.. _URDF-Tutorials-repo: https://github.com/ros/urdf_tutorial
