# Copyright (c) 2014, Ruslan Baratov
# All rights reserved.

cmake_minimum_required(VERSION 3.0)
project(HunterSimple)

### Hunter snapshot that will be used ###
include("cmake/HunterGate.cmake")
HunterGate(
    URL "https://github.com/ruslo/hunter/archive/v0.5.5.tar.gz"
    SHA1 "53cf9406dc858061e61617c11d7e19024ebfa774"
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
