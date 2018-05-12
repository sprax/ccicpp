cmake_minimum_required(VERSION 3.5)
project(stuff_getter)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_executable(main src/main.cpp)
add_executable(cli src/cli.cpp)
