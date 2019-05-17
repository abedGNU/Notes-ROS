*********************
Robot arm model
*********************

In this chapter a Robot arm model will be defined. Refer to Gazebo chapter for simulation.
We will continue the model started in the introductory chapter on urdf.
We will create a new package called ``scara_g3_description``.

::

  cd workspace/src/Robots

  catkin_create_pkg scara_g3_description geometry_msgs urdf rviz xacro

  cd scara_g3_description

  mkdir launch urdf meshes rviz scripts

Joints
========


Links
=======

Gripper
=========

Meshes
========

Transmissions
==============
A transmission macro is created:

.. code:: xml

  <xacro:macro name="transmission_block" params="joint_name">
    <transmission name="tran1">

       <type>transmission_interface/SimpleTransmission</type>

       <joint name="${joint_name}">
                   <hardwareInterface>hardware_interface/PositionJointInterface</hardwareInterface>
       </joint>

       <actuator name="motor1">
        <mechanicalReduction>1</mechanicalReduction>
       </actuator>

    </transmission>
  </xacro:macro>

  <xacro:transmission_block joint_name="joint_name1"/>
  <xacro:transmission_block joint_name="joint_name2"/>
  <xacro:transmission_block joint_name="joint_name3"/>

``transmission_interface/SimpleTransmission`` is the only interface supported.
The ``hardwareInterface`` could be position, velocity, or effort interfaces. In this case we choose ``PositionJointInterface``.
The hardware interface will be loaded by ``gazebo_ros_control`` plugin. Refer to Gazebo chapter for more information about this plugin.


Gazebo
========

Refer to Gazebo chapter.
