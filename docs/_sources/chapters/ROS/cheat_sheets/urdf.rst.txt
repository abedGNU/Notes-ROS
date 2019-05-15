*********************
3D Modeling URDF
*********************

``joint_state_publisher``: This tool is very useful when designing robot models using URDF. This package contains a node called joint_state_publisher, which reads the robot model description, finds all joints, and publishes joint values to all nonfixed joints using GUI sliders. The user can interact with each robot joint using this tool and can visualize using RViz. While designing URDF, the user can verify the rotation and translation of each joint using this tool.

``robot_state_publisher``: This package reads the current robot joint states and publishes the 3D poses of each robot link using the kinematics tree build from the URDF. The 3D pose of the robot is published as the tf (transform) ROS. The tf ROS publishes the relationship between the coordinates frames of a robot.

``kdl_parser``: Kinematic and Dynamics Library (KDL) is an ROS package that contains parser tools to build a KDL tree from the URDF representation. The kinematic tree can be used to publish the joint states and also to forward and inverse the kinematics of the robot


URDF
======

``link``: The ``link`` tag represents a single link of a robot. Using this tag, we can model a robot link and its properties. The modeling includes the size, the shape, and the color, and it can even import a 3D mesh to represent the robot link. We can also provide the dynamic properties of the link, such as the inertial matrix and the collision properties.

<link name="<name of the link>">
<inertial>...........</inertial>
  <visual> ............</visual>
  <collision>..........</collision>
</link>

``joint``: The joint tag represents a robot joint. We can specify the kinematics and the dynamics of the joint, and set the limits of the joint movement and its velocity. The joint tag supports the different types of joints, such as revolute, continuous, prismatic, fixed, floating, and planar.

<joint name="<name of the joint>">
  <parent link="link1"/>
  <child link="link2"/>

  <calibration .... />
  <dynamics damping ..../>
  <limit effort .... />
</joint>


``gazebo``: This tag is used when we include the simulation parameters of the Gazebo simulator inside the URDF. We can use this tag to include gazebo plugins, gazebo material properties, and so on. The following shows an example using gazebo tags:

<gazebo reference="link_1">
   <material>Gazebo/Black</material>
</gazebo>


Create package
================

catkin_create_pkg mastering_ros_robot_description_pkg roscpp tf geometry_msgs urdf rviz xacro

sudo apt-get install ros-kinetic-urdf
sudo apt-get install ros-kinetic-xacro


Urdf model
===========
pan_tilt.urdf

<?xml version="1.0"?>
<robot name="pan_tilt">
  <link name="base_link">
    <visual>
      <geometry>
      <cylinder length="0.01" radius="0.2"/>
      </geometry>
      <origin rpy="0 0 0" xyz="0 0 0"/>
      <material name="yellow">
        <color rgba="1 1 0 1"/>
      </material>
    </visual>
  </link>
  <joint name="pan_joint" type="revolute">
    <parent link="base_link"/>
    <child link="pan_link"/>
    <origin xyz="0 0 0.1"/>
    <axis xyz="0 0 1" />
  </joint>
  <link name="pan_link">
    <visual>
      <geometry>
      <cylinder length="0.4" radius="0.04"/>
      </geometry>
      <origin rpy="0 0 0" xyz="0 0 0.09"/>
      <material name="red">
        <color rgba="0 0 1 1"/>
      </material>
    </visual>
  </link>
  <joint name="tilt_joint" type="revolute">
    <parent link="pan_link"/>
    <child link="tilt_link"/>
    <origin xyz="0 0 0.2"/>
    <axis xyz="0 1 0" />
  </joint>
  <link name="tilt_link">
    <visual>
      <geometry>
  <cylinder length="0.4" radius="0.04"/>
      </geometry>
      <origin rpy="0 1.5 0" xyz="0 0 0"/>
      <material name="green">
        <color rgba="1 0 0 1"/>
      </material>
    </visual>
  </link>
</robot>


check_urdf pan_tilt.urdf

urdf_to_graphiz pan_tilt.urdf


rosrun xacro xacro pan_tilt.xacro --inorder > pan_tilt_generated.urdf


The transmission tag relates a joint to an actuator. It defines the type of transmission that we are using in a particular joint, as well as the type of motor and its parameters. It also defines the type of hardware interface we use when we interface with the ROS controllers.

<xacro:macro name="transmission_block" params="joint_name">
 <transmission name="tran1">
   <type>transmission_interface/SimpleTransmission</type>
   <joint name="${joint_name}">
     <hardwareInterface>PositionJointInterface</hardwareInterface>
   </joint>
   <actuator name="motor1">
     <hardwareInterface>PositionJointInterface</hardwareInterface>
     <mechanicalReduction>1</mechanicalReduction>
   </actuator>
 </transmission>
</xacro:macro> 
