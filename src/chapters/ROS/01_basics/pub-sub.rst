*********************
Publisher-Subscriber
*********************

Simple Publisher-Subscriber ROS program
=======================================

In this section we will make a package that contain 2 nodes. A publisher node (talker.cpp) and a subscriber node (listener.cpp).
These two source files should be created in the src folder of the package.

Append to the CMakeLists.txt of the package the following:

.. code-block:: cmake

  add_executable(talker src/talker.cpp)
  target_link_libraries(talker ${catkin_LIBRARIES})

  add_executable(listener src/listener.cpp)
  target_link_libraries(listener ${catkin_LIBRARIES})


This will create two executables, talker and listener, which by default will go into package directory of your devel space, located by default at ~/catkin\_ws/devel/lib/<package name>.

Publisher node
---------------

.. literalinclude:: ../code/01_pub-sub/talker.cpp
  :caption: Publisher
  :name: lstPublisher

Subscriber Node
--------------------

.. literalinclude:: ../code/01_pub-sub/listener.cpp
  :caption: Subscriber
  :name: lstSubscriber

Building nodes
--------------

Packages should be stored in workspace, if no work space is present, one should be created.
The following steps should be done:

	- Create and build a workspace
	- Source the package environment variable
	- Create a package
	- Copy or create node source file into the src folder of the package
	- Eventually create new messages and services
	- Add nodes, messages and services to the CMakeLists.txt of the package
	- Build the workspace

Listing.\ref{lstquickROS} show steps necessary to create in the home directory a workspace named catkin\_ws, create a package called first\_tutorial, and create two nodes in that package then build the workspace.

``label=lstquickROS listing/tutorial/quickROS``

This will create two executables, talker and listener, which by default will go into package directory of your devel space, located by default at ~/catkin\_ws/devel/lib/<package name>.

Run nodes
---------

.. code-block:: bash

  roscore

  rosrun beginner_tutorials talker      (C++)
  rosrun beginner_tutorials talker.py   (Python)

.. code-block:: bash

	rosrun beginner_tutorials listener     (C++)
	rosrun beginner_tutorials listener.py  (Python)


Useful ROS commands
-------------------

rosnode
rostopic
rosmsg
