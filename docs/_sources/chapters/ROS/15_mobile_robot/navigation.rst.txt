*******************
Navigation
*******************

Hokuyo laser scanner
======================

We will add a laser scanner to the mobile robot. The sensor behavior is already defined in gazebo as a plugin. The plugin that we will use is called ``libgazebo_ros_laser.so``.
We will create a new xacro file called ``hokuyo_laser.xacro``, we define a link, a joint and add the gazebo plugin:

.. code:: xml

  <gazebo reference="hokuyo_link">
  <material>Gazebo/Blue</material>
  <turnGravityOff>false</turnGravityOff>
  <sensor type="ray" name="head_hokuyo_sensor">
    <pose>${length/2} 0 0 0 0 0</pose>
    <visualize>false</visualize>
    <update_rate>40</update_rate>
    <ray>
      <scan>
        <horizontal>
          <samples>720</samples>
          <resolution>1</resolution>
          <min_angle>-1.570796</min_angle>
          <max_angle>1.570796</max_angle>
        </horizontal>
      </scan>
      <range>
        <min>0.10</min>
        <max>10.0</max>
        <resolution>0.001</resolution>
      </range>
    </ray>
    <plugin name="gazebo_ros_head_hokuyo_controller" filename="libgazebo_ros_laser.so">
      <topicName>/scan</topicName>
      <frameName>hokuyo_link</frameName>
    </plugin>
  </sensor>
  </gazebo>

Create a new xacro file called ``mobile_robot_with_laser.xacro``, where we include the mobile robot definition and the laser macro:

.. code:: xml

  <?xml version="1.0"?>
  <robot name="mobile_robot"
    xmlns:xacro="http://www.ros.org/wiki/xacro">

    <!-- ================================================================================================================= -->
    <!-- INCLUDE FILES -->
    <xacro:include filename="$(find mobile_car_description)/urdf/mobile_robot.xacro" />
    <xacro:include filename="$(find mobile_car_description)/urdf/laser/hokuyo_laser.xacro" />

    <!-- ================================================================================================================= -->
     <!-- laser -->
    <hokuyo_laser name="hokuyo_link" parent="body_link" length="${hokuyo_length}" width="${hokuyo_width}" height="${hokuyo_height}" x="${hokuyo_x}" y="${hokuyo_y}" z="${hokuyo_z}" mass="${hokuyo_mass}"/>

    <!-- ================================================================================================================= -->

  </robot>

Open the robot in gazebo: ::

  roslaunch mobile_car_gazebo mobile_car_gazebo.launch model:='$(find mobile_car_description)/urdf/mobile_robot_with_laser.xacro'

Using ``rostopic list`` we can see the topic called ``/scan`` is being published. ``rostopic echo /scan`` to see the laser scanner value.


Add camera
============

.. code:: xml

  <gazebo reference="camera_link">
      <sensor type="camera" name="camera1">
        <update_rate>30.0</update_rate>
        <camera name="head">
          <horizontal_fov>1.3962634</horizontal_fov>
          <image>
            <width>800</width>
            <height>800</height>
            <format>R8G8B8</format>
          </image>
          <clip>
            <near>0.02</near>
            <far>300</far>
          </clip>
          <noise>
            <type>gaussian</type>
            <!-- Noise is sampled independently per pixel on each frame.
                 That pixel's noise value is added to each of its color
                 channels, which at that point lie in the range [0,1]. -->
            <mean>0.0</mean>
            <stddev>0.007</stddev>
          </noise>
        </camera>
        <plugin name="camera_controller" filename="libgazebo_ros_camera.so">
          <alwaysOn>true</alwaysOn>
          <updateRate>0.0</updateRate>
          <cameraName>robot/camera1</cameraName>
          <imageTopicName>image_raw</imageTopicName>
          <cameraInfoTopicName>camera_info</cameraInfoTopicName>
          <frameName>camera_link</frameName>
          <hackBaseline>0.07</hackBaseline>
          <distortionK1>0.0</distortionK1>
          <distortionK2>0.0</distortionK2>
          <distortionK3>0.0</distortionK3>
          <distortionT1>0.0</distortionT1>
          <distortionT2>0.0</distortionT2>
        </plugin>
      </sensor>
    </gazebo>

Show image: ::

  rosrun image_view image_view image:=/robot/camera1/image_raw


Loading maps in gazebo
=========================


Teleoperate the robot
=======================
