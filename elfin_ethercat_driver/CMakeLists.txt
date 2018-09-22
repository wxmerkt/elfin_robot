cmake_minimum_required(VERSION 2.8.3)
project(elfin_ethercat_driver)

## Find catkin macros and libraries
find_package(catkin REQUIRED COMPONENTS
  controller_manager
  diagnostic_updater
  elfin_robot_msgs
  hardware_interface
  joint_limits_interface
  roscpp
  rospy
  sensor_msgs
  soem
  tf
  trajectory_msgs
  transmission_interface
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
 INCLUDE_DIRS include
 LIBRARIES elfin_ethercat_driver
 CATKIN_DEPENDS controller_manager diagnostic_updater hardware_interface joint_limits_interface roscpp rospy sensor_msgs soem tf trajectory_msgs transmission_interface elfin_robot_msgs
)

###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
# include_directories(include)
include_directories(
  ${catkin_INCLUDE_DIRS}
  ${soem_INCLUDE_DIRS}/soem
  include
)

#add_executable(elfin_ethercat_driver src/elfin_ethercat_driver.cpp)
#target_link_libraries(elfin_ethercat_driver ${catkin_LIBRARIES})
#add_dependencies(elfin_ethercat_driver ${catkin_EXPORTED_TARGETS})

# Declare a C++ library
add_library(elfin_ethercat_driver
  src/elfin_ethercat_driver.cpp
  src/elfin_ethercat_client.cpp
  src/elfin_ethercat_io_client.cpp
  src/elfin_ethercat_manager.cpp
  include/${PROJECT_NAME}/elfin_ethercat_driver.h
  include/${PROJECT_NAME}/elfin_ethercat_client.h
  include/${PROJECT_NAME}/elfin_ethercat_io_client.h
  include/${PROJECT_NAME}/elfin_ethercat_manager.h
)

# Add cmake target dependencies of the library
# as an example, code may need to be generated before libraries
# either from message generation or dynamic reconfigure
add_dependencies(elfin_ethercat_driver ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

# Declare a C++ executable
add_executable(elfin_ethercat_driver_node src/elfin_ethercat_driver.cpp
    src/elfin_ethercat_client.cpp
    src/elfin_ethercat_io_client.cpp
    src/elfin_ethercat_manager.cpp)

# Add cmake target dependencies of the executable
# same as for the library above
add_dependencies(elfin_ethercat_driver_node ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

# Specify libraries to link a library or executable target against
target_link_libraries(elfin_ethercat_driver
  ${catkin_LIBRARIES}
)

target_link_libraries(elfin_ethercat_driver_node
  ${catkin_LIBRARIES}
)


#############
## Install ##
#############

# all install targets should use catkin DESTINATION variables
# See http://ros.org/doc/api/catkin/html/adv_user_guide/variables.html

## Mark executable scripts (Python etc.) for installation
## in contrast to setup.py, you can choose the destination
# install(PROGRAMS
#   scripts/my_python_script
#   DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

# Mark executables and/or libraries for installation
install(TARGETS elfin_ethercat_driver elfin_ethercat_driver_node
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

# Mark cpp header files for installation
install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.h"
  PATTERN ".svn" EXCLUDE
)

## Mark other files for installation (e.g. launch and bag files, etc.)
# install(FILES
#   # myfile1
#   # myfile2
#   DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
# )

#############
## Testing ##
#############

## Add gtest based cpp test target and link libraries
# catkin_add_gtest(${PROJECT_NAME}-test test/test_elfin_ethercat_driver.cpp)
# if(TARGET ${PROJECT_NAME}-test)
#   target_link_libraries(${PROJECT_NAME}-test ${PROJECT_NAME})
# endif()

## Add folders to be run by python nosetests
# catkin_add_nosetests(test)



