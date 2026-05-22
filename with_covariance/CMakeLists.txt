cmake_minimum_required(VERSION 3.5)
project(with_covariance)

find_package(ament_cmake_auto REQUIRED)
ament_auto_find_build_dependencies()

ament_auto_add_library(with_covariance_node src/with_covariance.cpp)

# auto-executable from node
rclcpp_components_register_node(with_covariance_node
  PLUGIN "with_covariance::WithCovariance"
  EXECUTABLE with_covariance)

install(DIRECTORY example
        DESTINATION share/${PROJECT_NAME})

ament_auto_package()
