# Copyright (c) Huawei Technologies Co., Ltd. 2019. All rights reserved.

# CMake lowest version requirement
cmake_minimum_required(VERSION 3.5.1)

# project information
project(DECODE_AND_RESIZE)

# Compile options
add_compile_options(-std=c++11)

add_definitions(-DENABLE_DVPP_INTERFACE)

# Specify target generation path
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY  "../../../out")
set(CMAKE_CXX_FLAGS_DEBUG "-fPIC -O0 -g -Wall")
set(CMAKE_CXX_FLAGS_RELEASE "-fPIC -O2 -Wall")
set(CMAKE_BUILD_TYPE Release)

if (NOT DEFINED ENV{INSTALL_DIR})
    MESSAGE(FATAL_ERROR "Not Defined INSTALL_DIR")
endif()

# Header path
include_directories(
	$ENV{INSTALL_DIR}/runtime/include/
        ../inc/
)

# add host lib path
link_directories(
	$ENV{INSTALL_DIR}/runtime/lib64/stub
)

add_executable(main
        main.cpp
        inference.cpp
        model_process.cpp
        dvpp_preProcess.cpp)

target_link_libraries(main
        ascendcl acl_cblas acl_dvpp stdc++ opencv_core opencv_imgproc opencv_imgcodecs)

install(TARGETS main DESTINATION ${CMAKE_RUNTIME_OUTPUT_DIRECTORY})
