****************************
Point Cloud Library (PCL_)
****************************

.. note::
    References: Ros Industrial training. PCL_Tools_.

PCL tools
===========

Publish point cloud on the topic ``topic_cloud_pcd``::

  roscore
  rosrun pcl_ros pcd_to_pointcloud table.pcd 0.1 _frame_id:=map cloud_pcd:=topic_cloud_pcd
  rosrun rviz rviz

In order to see the point cloud data, add ``PointCloud2`` display and select the topic ``topic_cloud_pcd``.

Install ``pcl-tools`` ::

  sudo apt install pcl-tools

Point cloud data can be viewed using the command line ``pcl_viewer``, this command is not part of ``ROS``, so no need to run ``roscore``. ::

  pcl_viewer table.pcd

Downsample the point cloud using the ``pcl_voxel_grid``
--------------------------------------------------------
Downsample the original point cloud using a voxel grid with a grid size of (0.05,0.05,0.05).
In a voxel grid, all points in a single grid cube are replaced with a single point at the center of the voxel.
This is a common method to simplify overly complex/detailed sensor data, to speed up processing steps. ::

  pcl_voxel_grid table.pcd table_downsampled.pcd -leaf 0.05,0.05,0.05
  pcl_viewer table_downsampled.pcd

Segmentation ``pcl_sac_segmentation_plane``
-----------------------------------------------
Extract the table surface, find the largest plane and extract points that belong to that plane (within a given threshold). ::

  pcl_sac_segmentation_plane table_downsampled.pcd only_table.pcd -thresh 0.01
  pcl_viewer only_table.pcd

Extract the largest point-cluster not belonging to the table. ::

  pcl_sac_segmentation_plane table.pcd object_on_table.pcd -thresh 0.01 -neg 1
  pcl_viewer object_on_table.pcd

Remove outliers ``pcl_outlier_removal``
-------------------------------------------
For this example, a statistical method will be used for removing outliers. This is useful to clean up noisy sensor data, removing false artifacts before further processing. ::

  pcl_outlier_removal table.pcd table_outlier_removal.pcd -method statistical
  pcl_viewer table_outlier_removal.pcd

Compute the normals ``pcl_normal_estimation``
-------------------------------------------------
This example estimates the local surface normal (perpendicular) vectors at each point.
For each point, the algorithm uses nearby points (within the specified radius) to fit a plane and calculate the normal vector.
Zoom in to view the normal vectors in more detail. ::

  pcl_normal_estimation only_table.pcd table_normals.pcd -radius 0.1
  pcl_viewer table_normals.pcd -normals 10

Mesh a point cloud ``pcl_marching_cubes_reconstruction``
-----------------------------------------------------------
Point cloud data is often unstructured, but sometimes processing algorithms need to operate on a more structured surface mesh.
This example uses the “marching cubes” algorithm to construct a surface mesh that approximates the point cloud data. ::

  pcl_marching_cubes_reconstruction table_normals.pcd table_mesh.vtk -grid_res 20
  pcl_viewer table_mesh.vtk

PLC basics
=============

Data type
-----------

PCL ROS API
=============
::

  catkin_create_pkg pcl_test_pkg pcl_conversions pcl_ros pcl_msgs sensor_msgs

.. _PCL: http://www.pointclouds.org/
.. _PCL_Tools: https://github.com/PointCloudLibrary/pcl/tree/master/tools
