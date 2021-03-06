cmake_minimum_required(VERSION 2.8.3)
project(topic_throttle)

find_package(catkin REQUIRED COMPONENTS
  nodelet_topic_tools
  nodelet
  roscpp
  std_msgs
  topic_tools
)

###################################
## catkin specific configuration ##
###################################
## The catkin_package macro generates cmake config files for your package
## Declare things to be passed to dependent projects
## INCLUDE_DIRS: uncomment this if you package contains header files
## LIBRARIES: libraries you create in this project that dependent projects also need
## CATKIN_DEPENDS: catkin_packages dependent projects also need
## DEPENDS: system dependencies of this project that dependent projects also need
catkin_package(
#  INCLUDE_DIRS include
  LIBRARIES queue_throttle
#  CATKIN_DEPENDS other_catkin_pkg
#  DEPENDS system_lib
)

include_directories(
  ${catkin_INCLUDE_DIRS}
)

add_library(topic_throttle
  src/queue_throttle.cpp
  test/imu_nodelet_throttle.cpp  # TODO(lucasw) only build in test mode, in separate lib
)
target_link_libraries(topic_throttle ${catkin_LIBRARIES})

add_executable(queue_throttle_node src/queue_throttle_node.cpp)
add_dependencies(queue_throttle_node topic_throttle)
target_link_libraries(queue_throttle_node ${catkin_LIBRARIES})

# TODO(lucasw) only build this in test build mode
add_executable(imu_throttle test/imu_throttle.cpp)
target_link_libraries(imu_throttle ${catkin_LIBRARIES})
