# rock1
# A rock solid project with enforced style, testing and static analysis
# Copyright 2018, Mario Costa
cmake_minimum_required(VERSION 3.1)
project(rock1 VERSION 1.0.0 LANGUAGES C CXX)
set(PROJECT_DESCRIPTION   "A rock solid project with enforced style, testing and static analysis")
set(PROJECT_AUTHOR        "Mario Costa")
set(PROJECT_URL           "https://github.com/practicalci/rock1")


# ------------------------------------------------------------------------------
# Add external cmake modules to project.
# Add find_<package_name> files to cmake-modules directory.
# ------------------------------------------------------------------------------

list(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake-modules")

# Download automatically, you can also just copy the conan.cmake file
if(NOT EXISTS "${CMAKE_BINARY_DIR}/conan.cmake")
   message(STATUS "Downloading conan.cmake from https://github.com/conan-io/cmake-conan")
   file(DOWNLOAD "https://raw.githubusercontent.com/conan-io/cmake-conan/v0.13/conan.cmake"
                 "${CMAKE_BINARY_DIR}/conan.cmake")
endif()

include(${CMAKE_BINARY_DIR}/conan.cmake)

conan_cmake_run(CONANFILE conanfile.txt  # or relative build/conanfile.txt
                BASIC_SETUP CMAKE_TARGETS
                BUILD missing)

# Build flags
set (CMAKE_CXX_STANDARD 11)


# Enable only coverage in Debug mode, avoid adding debug symbols for Release mode.
if (CMAKE_BUILD_TYPE STREQUAL "Debug")
  include(CodeCoverage)
  APPEND_COVERAGE_COMPILER_FLAGS()
endif()

# ------------------------------------------------------------------------------
# Setup Compiler Flags
# ------------------------------------------------------------------------------

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall")

# make sure you want to use -march=native
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -O3")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wextra -Werror -pedantic")


# ------------------------------------------------------------------------------
# Features
# ------------------------------------------------------------------------------
add_subdirectory(src)



# ------------------------------------------------------------------------------
# Setup tests and checks
# ------------------------------------------------------------------------------

# (catch must be configured to generate a main class per test).


enable_testing()
set(CTEST_OUTPUT_ON_FAILURE 1)

# Enable only coverage in Debug mode, avoid adding debug symbols for Release mode.
if (CMAKE_BUILD_TYPE STREQUAL "Debug")
  setup_target_for_coverage_gcovr_html( NAME ${PROJECT_NAME}_cov EXECUTABLE ${PROJECT_NAME}_test )
endif()

add_subdirectory(test)
add_subdirectory(doc)
