cmake_minimum_required(VERSION 3.5)
project(rosbridge_msgs)

# CMake >= 3.27: Set CMake policy CMP0148 on finding Python to avoid:
# "Policy CMP0148 is not set: The FindPythonInterp and FindPythonLibs modules
# are removed."
if(CMAKE_MINOR_VERSION VERSION_GREATER_EQUAL 27)
  cmake_policy(SET CMP0148 OLD)
endif()

find_package(ament_cmake_ros REQUIRED)
find_package(builtin_interfaces REQUIRED)
find_package(rosidl_default_generators REQUIRED)

if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

rosidl_generate_interfaces(${PROJECT_NAME}
  msg/ConnectedClient.msg
  msg/ConnectedClients.msg
  DEPENDENCIES builtin_interfaces
)

ament_export_dependencies(rosidl_default_runtime)

ament_package()
