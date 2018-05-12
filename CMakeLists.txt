cmake_minimum_required(VERSION 3.8)
project(stuff_getter)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

include(${CMAKE_ROOT}/Modules/ExternalProject.cmake)

# Single download(git clone)
ExternalProject_Add(
    googletest-master
    DOWNLOAD_DIR "externals/gtest" # The only dir option which is required
    GIT_REPOSITORY https://github.com/google/googletest.git
    TIMEOUT 10
    LOG_DOWNLOAD ON
    # Disable all other steps
    CONFIGURE_COMMAND ""
    BUILD_COMMAND ""
    INSTALL_COMMAND ""
)

# Build gtest from existing sources
ExternalProject_Add(
    gtest
    DOWNLOAD_COMMAND "" # No download required
    SOURCE_DIR "externals/gtest" # Use specific source dir
    PREFIX "externals" # But use prefix for compute other dirs
    INSTALL_COMMAND ""
    LOG_CONFIGURE ON
    LOG_BUILD ON
)

# build gtest after download
add_dependencies(gtest externals/gtest)

# Build gmock from existing sources
ExternalProject_Add(
    gmock
    DOWNLOAD_COMMAND "" # No download required
    SOURCE_DIR "externals/googlemock" # Use specific source dir
    PREFIX "externals" # But use prefix for compute other dirs
    INSTALL_COMMAND ""
    LOG_CONFIGURE ON
    LOG_BUILD ON
)

# find_package(CLI11 CONFIG)

add_executable(main src/main.cpp)
add_executable(cli src/cli.cpp)

set(GTEST_SRC_FILES
	tests/test_dir_poll.cc
)

foreach(_file ${GTEST_SRC_FILES})
    get_filename_component(_name ${_file} NAME_WE)
    add_executable(${_name} ${_file})
    target_link_libraries(${_name} gflags gtest gtest_main ${CMAKE_THREAD_LIBS_INIT})
    add_test(${_name} ${_name})
    set_tests_properties(${_name} PROPERTIES TIMEOUT 70)
endforeach()
