cmake_minimum_required(VERSION 2.6.3)

project(rtt_soem_robotiq)
# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
#set(ROS_BUILD_TYPE RelWithDebInfo)

 
# Do setup in case of ros package, If ROS_ROOT is set, it is                                                                                                 # recommended to use RTT/OCL through the ros packages.    


find_package(catkin REQUIRED message_generation rtt_roscomm)
find_package(OROCOS-RTT QUIET)
if (NOT OROCOS-RTT_FOUND)
  message (FATAL_ERROR "\nCould not find Orocos. Please use the shell command\n 'source orocos_toolchain/env.sh' and then run cmake again.")
endif()

# Defines the orocos_* cmake macros. See that file for additional
# documentation.
include( ${OROCOS-RTT_USE_FILE_PATH}/UseOROCOS-RTT.cmake )
add_message_files( FILES
    FingerMsg.msg
    RobotiqMsg.msg
)
  
generate_messages()
                                                                                                   



ros_generate_rtt_typekit(rtt_soem_robotiq)

find_package(soem REQUIRED)
include_directories(${soem_INCLUDE_DIRS})
orocos_use_package(soem_master)

orocos_plugin(soem_robotiq src/soem_robotiq_drivers.cpp src/soem_robotiq_3Finger.cpp)

add_dependencies(soem_robotiq ${PROJECT_NAME}_generate_messages_cpp)
orocos_generate_package()

