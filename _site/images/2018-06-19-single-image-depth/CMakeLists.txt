cmake_minimum_required(VERSION 2.8.3)
project(mabi_load_identification)

# Set compiler flags
set(CMAKE_CXX_FLAGS "-std=c++11 -DLC_ENABLE_LOGGER ${CMAKE_CXX_FLAGS}")


find_package(catkin REQUIRED COMPONENTS
  any_node
  any_measurements
  any_measurements_ros
  mabi_description
  mabi_model
  mabi_roco
  cosmo_ros
  message_logger
  parameter_handler_ros
  roco
  rocoma_ros
  mabi_joint_actuator
  mabi_joint_actuator_msgs
  mabi_joint_actuator_cosmo
  mabi_lowlevel_controller
  mabi_trajectory_generator
  mabi_control_utils
  sensor_msgs
  signal_logger
  signal_logger_ros
  signal_logger_std
  kindr_ros

  message_generation
  message_runtime
)

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES ${PROJECT_NAME}
  CATKIN_DEPENDS
    any_node
    any_measurements
    any_measurements_ros
    mabi_description
    mabi_model
  	mabi_roco
    cosmo_ros
  	message_logger
  	parameter_handler_ros
  	roco
  	rocoma_ros
    mabi_joint_actuator
    mabi_joint_actuator_msgs
  	mabi_joint_actuator_cosmo
    mabi_lowlevel_controller
    mabi_trajectory_generator
    mabi_control_utils
    sensor_msgs
  	signal_logger
  	signal_logger_ros
  	signal_logger_std
  	kindr_ros

    message_generation
    message_runtime
  DEPENDS
)


include_directories(
  include
  ${catkin_INCLUDE_DIRS}
)

add_library(${PROJECT_NAME}
  src/mabiLoadIdentification.cpp
)

add_dependencies(${PROJECT_NAME} ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
target_link_libraries(${PROJECT_NAME}
  ${catkin_LIBRARIES}
)

# Declare a C++ executable
add_executable(${PROJECT_NAME}_node src/${PROJECT_NAME}_node.cpp)

add_dependencies(${PROJECT_NAME}_node ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
target_link_libraries(${PROJECT_NAME}_node
  ${PROJECT_NAME}
  ${catkin_LIBRARIES}
)

############
## INSTALL ##
#############

install(
  TARGETS ${PROJECT_NAME} ${PROJECT_NAME}_node
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
)

install(
  DIRECTORY param launch
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)
