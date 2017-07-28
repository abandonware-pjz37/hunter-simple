# Copyright (c) 2014-2016, Ruslan Baratov
# All rights reserved.

cmake_minimum_required(VERSION 3.0)

### Hunter snapshot that will be used ###
include("cmake/HunterGate.cmake")
HunterGate(
    URL "https://github.com/ruslo/hunter/archive/v0.19.51.tar.gz"
    SHA1 "d238dc1dd4db83e45a592f96fdb95d17c688600a"
)
project(HunterSimple)

### Download dependencies ###
hunter_add_package(GTest)

### Find dependencies ###
find_package(GTest CONFIG REQUIRED) # GTest::main

### Targets ###
add_executable(simple simple.cpp)
target_link_libraries(simple PUBLIC GTest::main)

### Testing ###
enable_testing()

if(IOS)
  # Extra cmake modules needed for iOS testing:
  # * https://github.com/ruslo/sugar/tree/master/cmake/core#sugar_add_ios_gtest
  add_test(
      NAME SimpleTest COMMAND "${CMAKE_COMMAND}" -E echo "iOS testing ignored"
  )
elseif(ANDROID)
  # Building .apk, testing: https://github.com/forexample/android-cmake
  add_test(
      NAME SimpleTest
      COMMAND "${CMAKE_COMMAND}" -E echo "Android testing ignored"
  )
else()
  add_test(NAME SimpleTest COMMAND simple)
endif()
