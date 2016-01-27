# Copyright (c) 2014, Ruslan Baratov
# All rights reserved.

cmake_minimum_required(VERSION 3.0)

### Hunter snapshot that will be used ###
include("cmake/HunterGate.cmake")
HunterGate(
    URL "https://github.com/ruslo/hunter/archive/v0.12.40.tar.gz"
    SHA1 "0ccc3f3fd571676a1804723984598f9f90a4d6bc"
)

project(HunterSimple)

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
