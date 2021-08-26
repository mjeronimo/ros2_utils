cmake_minimum_required(VERSION 3.5)
project(ros2_utils)

find_package(ament_cmake REQUIRED)
find_package(cv_bridge REQUIRED)
find_package(lifecycle_msgs REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rclcpp_lifecycle REQUIRED)
find_package(sensor_msgs REQUIRED)

set(dependencies
  cv_bridge
  lifecycle_msgs
  rclcpp
  rclcpp_lifecycle
  sensor_msgs
)

if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic -Werror -Wdeprecated -fPIC)
endif()

include_directories(
  include
)

set(library_name ${PROJECT_NAME}_core)

add_library(${library_name} SHARED
  src/cv_utils.cpp
  src/lifecycle_service_client.cpp
  src/node_thread.cpp
  src/node_utils.cpp
  src/param_utils.cpp
  src/string_utils.cpp
)

ament_target_dependencies(${library_name} ${dependencies})

install(DIRECTORY include/
  DESTINATION include/
)

install(TARGETS ${library_name}
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  find_package(ament_cmake_pytest REQUIRED)
  find_package(launch_testing_ament_cmake REQUIRED)
  ament_lint_auto_find_test_dependencies()

  find_package(ament_cmake_gtest REQUIRED)
endif()

ament_export_include_directories(include)
ament_export_libraries(${library_name})
ament_export_dependencies(${dependencies})

ament_package()
