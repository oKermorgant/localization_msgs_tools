cmake_minimum_required(VERSION 3.5)
project(pose_to_tf)

find_package(ament_cmake_auto REQUIRED)
ament_auto_find_build_dependencies()

ament_auto_add_library(pose_to_tf_node SHARED src/pose_to_tf.cpp)

# auto-executable from node
rclcpp_components_register_node(pose_to_tf_node
  PLUGIN "pose_to_tf::Pose2TF"
  EXECUTABLE pose_to_tf)

ament_auto_package(USE_SCOPED_HEADER_INSTALL_DIR)
