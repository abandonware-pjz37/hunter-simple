# Copyright (c) 2014, Ruslan Baratov
# All rights reserved.

cmake_minimum_required(VERSION 3.0)
project(HunterSimple)

### Hunter snapshot that will be used ###
include("cmake/HunterGate.cmake")
HunterGate(
    URL "https://github.com/ruslo/hunter/archive/v0.5.4.tar.gz"
    SHA1 "ccf0b8645b0833d3025ed0952df466ba5e683de8"
)

### Download dependencies ###
hunter_add_package(GTest)

### Find dependencies ###
find_package(GTest CONFIG REQUIRED NO_CMAKE_BUILDS_PATH) # GTest::main

### Targets ###
add_executable(simple simple.cpp)
target_link_libraries(simple GTest::main)

### Testing ###
enable_testing()
add_test(SimpleTest simple)
