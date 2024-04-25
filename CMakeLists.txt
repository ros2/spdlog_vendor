cmake_minimum_required(VERSION 3.10)
project(spdlog_vendor CXX)

find_package(ament_cmake REQUIRED)
find_package(ament_cmake_vendor_package REQUIRED)

find_package(spdlog 1.5.0 QUIET)

# While most CMake projects build shared libraries by default, it appears that
# spdlog needs to be told to do so explicitly.
set(BUILD_SHARED_LIBS TRUE CACHE BOOL "Build shared libraries")

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  set(CMAKE_CXX_FLAGS "-Wno-deprecated-declarations ${CMAKE_CXX_FLAGS}")
endif()

ament_vendor(spdlog_vendor
  SATISFIED ${spdlog_FOUND}
  VCS_URL https://github.com/gabime/spdlog.git
  VCS_VERSION v1.12.0
  CMAKE_ARGS
    -DSPDLOG_BUILD_TESTS:BOOL=OFF
    -DSPDLOG_BUILD_EXAMPLE:BOOL=OFF
    "-DCMAKE_CXX_FLAGS=${CMAKE_CXX_FLAGS}"
)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  list(APPEND AMENT_LINT_AUTO_EXCLUDE
    ament_cmake_cppcheck
  )
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
