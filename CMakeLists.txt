# Copyright (c) 2014, Ruslan Baratov
# All rights reserved.

cmake_minimum_required(VERSION 3.0)
project(HunterSimple)

### Download dependencies ###
include(HunterGate.cmake)
hunter_add_package(GTest)

### Find dependencies ###
find_package(GTest CONFIG REQUIRED NO_CMAKE_BUILDS_PATH) # GTest::main

### Targets ###
add_executable(simple simple.cpp)
target_link_libraries(simple GTest::main)

### Testing ###
enable_testing()
add_test(SimpleTest simple)
