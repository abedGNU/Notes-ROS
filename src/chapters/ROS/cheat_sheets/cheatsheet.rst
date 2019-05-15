*******************
Cheat Sheet
*******************

source /opt/ros/kinetic/setup.bash

Create workspace
------------------

mkdir -p ~/catkin_ws/src

cd ~/catkin_ws/src

catkin_init_workspace

cd ~/catkin_ws

catkin_make

echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc

source ~/.bashrc

Create package
----------------
catkin_create_pkg package_name [dependency1] [dependency2]

catkin_create_pkg demo_package roscpp std_msgs actionlib actionlib_msgs

ROS navigation
---------------

rosnode list
rosnode info demo_topic_publisher

rostopic echo /topicname

rostopic type /topicname


rosservice list

rosservice type /demo_service

rosservice info /demo_service

rosservice call /service [args...]
