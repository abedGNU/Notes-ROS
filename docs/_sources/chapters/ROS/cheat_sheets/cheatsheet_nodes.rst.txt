*********************
Computational graph
*********************

Topics and messages
=====================

Publisher
---------

ros::NodeHandle node_obj;
ros::Publisher number_publisher =  node_obj.advertise<std_msgs::Int32>("/numbers",10);
std_msgs::Int32 msg;
number_publisher.publish(msg);

Subscriber
-----------

ros::NodeHandle node_obj;
ros::Subscriber number_subscriber = node_obj.subscribe("/numbers",10,number_callback);
//This is an infinite loop in which the node will wait in this step
ros::spin();

//Callback of the topic /numbers
void number_callback(const std_msgs::Int32::ConstPtr& msg)
{
	ROS_INFO("Recieved  [%d]",msg->data);
}


Services
==========

demo_srv.srv

  # Request
  string in
  ---
  # Response
  string out

Service server
----------------
#include "ros/ros.h"
#include "mastering_ros_demo_pkg/demo_srv.h"
#include <iostream>
#include <sstream>
using namespace std;

ros::init(argc, argv, "demo_service_server");
ros::NodeHandle n;

ros::ServiceServer service = n.advertiseService("demo_service", demo_service_callback);
ros::spin();

bool demo_service_callback(mastering_ros_demo_pkg::demo_srv::Request  &req,
         mastering_ros_demo_pkg::demo_srv::Response &res) {
  std::stringstream ss;
  ss << "Received  Here";
  res.out = ss.str();

  ROS_INFO("From Client  [%s], Server says [%s]",req.in.c_str(),res.out.c_str());
  return true;
}

Service client
----------------

ros::NodeHandle n;
ros::Rate loop_rate(10);

ros::ServiceClient client = n.serviceClient<mastering_ros_demo_pkg::demo_srv>("demo_service");

mastering_ros_demo_pkg::demo_srv srv;
std::stringstream ss;
ss << "Sending from Here";
srv.request.in = ss.str();

if (client.call(srv)){
  ROS_INFO("From Client  [%s], Server says [%s]",srv.request.in.c_str(),srv.response.out.c_str());
}
else{
  ROS_ERROR("Failed to call service");
  return 1;
}

actionlib
===========
Demo_action.action

#goal definition
int32 count
---
#result definition
int32 final_count
---
#feedback
int32 current_number

Action server
----------------

Action client
--------------
