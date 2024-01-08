cmake_minimum_required(VERSION 3.5)
project(rosbridge_server)

# CMake >= 3.27 + ROS2 Humble:
# Set CMake policy CMP0148 on finding Python to avoid:
# "Policy CMP0148 is not set: The FindPythonInterp and FindPythonLibs modules
# are removed."
if(CMAKE_MINOR_VERSION VERSION_GREATER_EQUAL 27)
  cmake_policy(SET CMP0148 OLD)
endif()

find_package(ament_cmake_ros REQUIRED)
find_package(ament_cmake_core REQUIRED)
find_package(ament_cmake_python REQUIRED)

ament_python_install_package(
  ${PROJECT_NAME} PACKAGE_DIR "src/${PROJECT_NAME}")

ament_package()

install(PROGRAMS
  scripts/rosbridge_websocket.py
  scripts/rosbridge_websocket
  DESTINATION lib/${PROJECT_NAME}
)

install(FILES
  launch/rosbridge_websocket_launch.xml
  DESTINATION share/${PROJECT_NAME}/launch
)

if(BUILD_TESTING)
  find_package(launch_testing_ament_cmake REQUIRED)
  add_launch_test(test/websocket/advertise_action.test.py)
  add_launch_test(test/websocket/advertise_action_feedback.test.py)
  add_launch_test(test/websocket/advertise_service.test.py)
  add_launch_test(test/websocket/call_service.test.py)
  add_launch_test(test/websocket/send_action_goal.test.py)
  add_launch_test(test/websocket/smoke.test.py)
  add_launch_test(test/websocket/transient_local_publisher.test.py)
  add_launch_test(test/websocket/best_effort_publisher.test.py)
  add_launch_test(test/websocket/multiple_subscribers_raw.test.py)
endif()
