# Copyright (c) 2014, Ruslan Baratov
# All rights reserved.

cmake_minimum_required(VERSION 3.0)
project(HunterSimple)

### Hunter snapshot that will be used ###
include("cmake/HunterGate.cmake")
HunterGate(
    URL "https://github.com/ruslo/hunter/archive/v0.5.10.tar.gz"
    SHA1 "d534c6f3ab9c53c325ca4d3607f65bbf981913db"
)

### Download dependencies ###
hunter_add_package(GTest)

### Find dependencies ###
find_package(GTest CONFIG REQUIRED NO_CMAKE_BUILDS_PATH) # GTest::main

### Targets ###
add_executable(simple simple.cpp)
target_link_libraries(simple GTest::main)

### Testing ###
string(COMPARE EQUAL "${CMAKE_OSX_SYSROOT}" "iphoneos" is_ios)
if(NOT is_ios)
  enable_testing()
  add_test(SimpleTest simple)
else()
  # https://github.com/ruslo/sugar/tree/master/cmake/core#sugar_add_gtest
endif()
