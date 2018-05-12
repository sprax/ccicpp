cmake_minimum_required(VERSION 3.5)
project(stuff_getter)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# find_package(CLI11 CONFIG)

add_executable(main src/main.cpp)
add_executable(cli src/cli.cpp)

set(GTEST_SRC_FILES
	tests/test_dir_poll.cc
)

foreach(_file ${GTEST_SRC_FILES})
    get_filename_component(_name ${_file} NAME_WE)
    add_executable(${_name} ${_file})
    target_link_libraries(${_name} ice_cream_scooper gflags gtest gtest_main ${CMAKE_THREAD_LIBS_INIT})
    add_test(${_name} ${_name})
    set_tests_properties(${_name} PROPERTIES TIMEOUT 70)
endforeach()
