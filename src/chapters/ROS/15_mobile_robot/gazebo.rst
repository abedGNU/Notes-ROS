*******************
Gazebo
*******************
In order to simulate and control the robot with gazebo we need to add some tags to the urdf files.

Create a package that depend on the robot model, ``mobile_car_description`` called ``mobile_car_gazebo``: ::

  catkin_create_pkg mobile_car_gazebo gazebo_msgs gazebo_plugins gazebo_ros gazebo_ros_control mobile_car_description
  mkdir launch

Launch file
==============

In order to simulate the robot in Gazebo_ we need to open a world file. In this example we open the ``empty_world.world`` already defined in the package ``gazebo_ros`` with default values.

.. code:: xml

  <!-- We resume the logic in empty_world.launch -->
  <include file="$(find gazebo_ros)/launch/empty_world.launch">
    <arg name="world_name" value="worlds/empty_world.world"/> <!-- Note: the world_name is with respect to GAZEBO_RESOURCE_PATH environmental variable -->
    <arg name="paused" value="false"/>
    <arg name="use_sim_time" value="true"/>
    <arg name="gui" value="true"/>
    <arg name="recording" value="false"/>
    <arg name="debug" value="false"/>
  </include>

World files are ``SDF`` files (xml files).

Spawn robots
-------------------

There are two ways to launch (``spawn``) your URDF-based robot into Gazebo using roslaunch:

  * ROS Service Call Spawn Method:

      The first method keeps your robot's ROS packages more portable between computers and repository check outs. It allows you to keep your robot's location relative to a ROS package path, but also requires you to make a ROS service call using a small (python) script.

  * Model Database Method:

      The second method allows you to include your robot within the .world file, which seems cleaner and more convenient but requires you to add your robot to the Gazebo model database by setting an environment variable.

Service call
^^^^^^^^^^^^^^^^^
This method uses a small python script called spawn_model to make a service call request to the gazebo_ros ROS node (named simply "gazebo" in the rostopic namespace) to add a custom URDF into Gazebo. The spawn_model script is located within the gazebo_ros package. You can use this script in the following way: ::

  rosrun gazebo_ros spawn_model -file `rospack find MYROBOT_description`/urdf/MYROBOT.urdf -urdf -x 0 -y 0 -z 1 -model MYROBOT

To see all of the available arguments for spawn_model including namespaces, trimesh properties, joint positions and RPY orientation run: ::

    rosrun gazebo_ros spawn_model -h

In launch file:

.. code:: xml

  <!-- Spawn a robot into Gazebo -->
  <node name="spawn_urdf" pkg="gazebo_ros" type="spawn_model" args="-file $(find baxter_description)/urdf/baxter.urdf -urdf -z 1 -model baxter" />

Or using xarco model:

.. code:: xml

  <!-- Convert an xacro and put on parameter server -->
  <param name="robot_description" command="$(find xacro)/xacro --inorder $(find pr2_description)/robots/pr2.urdf.xacro" />

  <!-- Spawn a robot into Gazebo -->
  <node name="spawn_urdf" pkg="gazebo_ros" type="spawn_model" args="-param robot_description -urdf -model pr2" />

Model Database Robot Spawn Method
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Complete launch file
------------------------

Create a launch file in the package ``mobile_car_gazebo`` called ``mobile_car_gazebo.launch``:

.. code:: xml

  <launch>
    <arg name="paused" default="false"/>
    <arg name="use_sim_time" default="true"/>
    <arg name="gui" default="true"/>
    <arg name="recording" default="false"/>
    <arg name="debug" default="false"/>

    <include file="$(find gazebo_ros)/launch/empty_world.launch">
      <arg name="debug" value="$(arg debug)" />
      <arg name="gui" value="$(arg gui)" />
      <arg name="paused" value="$(arg paused)"/>
      <arg name="use_sim_time" value="$(arg use_sim_time)"/>
      <arg name="recording" value="$(arg recording)"/>
    </include>

    <!-- Convert an xacro and put on parameter server -->
    <param name="robot_description" command="$(find xacro)/xacro --inorder '$(find mobile_car_description)/urdf/mobile_robot_with_laser.xacro'" />

    <node name="joint_state_publisher" pkg="joint_state_publisher" type="joint_state_publisher"></node>

    <node pkg="robot_state_publisher" type="robot_state_publisher" name="robot_state_publisher" output="screen">
      <param name="publish_frequency" type="double" value="50.0" />
    </node>

    <!-- Spawn a robot into Gazebo by running a python script called spawn_model-->
    <node name="urdf_spawner" pkg="gazebo_ros" type="spawn_model" respawn="false" output="screen" args="-urdf -model mobile_robot -param robot_description"/>

  </launch>


Run the launh file: ::

  roslaunch mobile_car_gazebo mobile_car_gazebo.launch


gazebo_ros_control
======================

In order to simulate the robot we need to add a controller.The plugin ``gazebo_ros_control`` plugin can load different libraries. In the following code, ``libgazebo_ros_control.so`` is loaded:

.. code:: xml

  <gazebo>
    <plugin name="gazebo_ros_control" filename="libgazebo_ros_control.so">
      <robotNamespace>/MYROBOT</robotNamespace>
      <robotSimType>gazebo_ros_control/DefaultRobotHWSim</robotSimType>
    </plugin>
  </gazebo>

The gazebo_ros_control <plugin> tag also has the following optional child elements:

  - ``<robotNamespace>`` The ROS namespace to be used for this instance of the plugin, defaults to robot name in URDF/SDF
  - ``<controlPeriod>`` The period of the controller update (in seconds), defaults to Gazebo's period
  - ``<robotParam>`` The location of the robot_description (URDF) on the parameter server, defaults to '/robot_description'
  - ``<robotSimType>`` The pluginlib name of a custom robot sim interface to be used (see below for more details), defaults to 'DefaultRobotHWSim'. The default behavior provides the following ``ros_control`` interfaces:

    * hardware_interface::JointStateInterface
    * hardware_interface::EffortJointInterface
    * hardware_interface::VelocityJointInterface - not fully implemented


Modifying robot model
=======================
Add the follwing plugin to the robot model in ``mobile_robot.xacro`` in the package ``mobile_car_description``:

.. code:: xml

  <!-- Differential drive controller  -->
  <gazebo>
    <plugin name="differential_drive_controller" filename="libgazebo_ros_diff_drive.so">

      <rosDebugLevel>Debug</rosDebugLevel>
      <publishWheelTF>false</publishWheelTF>
      <robotNamespace>/</robotNamespace>
      <publishTf>1</publishTf>
      <publishWheelJointState>false</publishWheelJointState>
      <alwaysOn>true</alwaysOn>
      <updateRate>100.0</updateRate>

      <leftJoint>left_wheel_joint</leftJoint>
      <rightJoint>right_wheel_joint</rightJoint>

      <wheelSeparation>${base_width}</wheelSeparation>
      <wheelDiameter>${2*wheel_radius}</wheelDiameter>
      <broadcastTF>1</broadcastTF>
      <wheelTorque>30</wheelTorque>
      <wheelAcceleration>1.8</wheelAcceleration>
      <commandTopic>cmd_vel</commandTopic>
      <odometryFrame>odom</odometryFrame>
      <odometryTopic>odom</odometryTopic>
      <robotBaseFrame>base_link</robotBaseFrame>

    </plugin>

  </gazebo>

Different parameters have to be add to the plugin. The most important is ``commandTopic`` where we tell the controller to listen to ``cmd_vel`` topic.

Teleoperation
===============

Rqt
------
Open rqt node, then use the plugin robot steering  to publish the robot speed on ``cmd_vel`` topic.

teleop_twist_keyboard package
-------------------------------

.. _Gazebo: http://gazebosim.org/tutorials
