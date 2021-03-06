cmake_minimum_required(VERSION 2.6.3)

project(lcsr_tf_tools)

set (CMAKE_CXX_STANDARD 11)

find_package(catkin REQUIRED tf geometry_msgs interactive_markers visualization_msgs message_generation std_msgs)

add_message_files(
  FILES StaticTransform.msg)

generate_messages(DEPENDENCIES geometry_msgs std_msgs)

catkin_package(INCLUDE_DIRS include LIBRARIES tf_hold)

include_directories(include)

include_directories(${catkin_INCLUDE_DIRS})

add_executable(interactive_transform_publisher src/interactive_transform_publisher.cpp)
target_link_libraries(interactive_transform_publisher ${catkin_LIBRARIES})

add_library(tf_relay src/tf_relay.cpp)
target_link_libraries(tf_relay ${catkin_LIBRARIES})
add_executable(tf_relay_node src/tf_relay_node.cpp)
target_link_libraries(tf_relay_node ${catkin_LIBRARIES} tf_relay)

add_library(tf_hold src/tf_hold.cpp)
set_target_properties(tf_hold PROPERTIES COMPILE_FLAGS "-std=c++0x")
target_link_libraries(tf_hold ${catkin_LIBRARIES})

add_executable(camera_info_tf_hold src/camera_info_tf_hold.cpp)
set_target_properties(tf_hold PROPERTIES COMPILE_FLAGS "-std=c++0x")
target_link_libraries(camera_info_tf_hold tf_hold)
