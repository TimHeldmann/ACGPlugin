cmake_minimum_required(VERSION 3.16)
project(  ACGPlugin DESCRIPTION "An plugin for MetaCG's CaGe tool, to track argument flow across a callgraph'")

include(FetchContent)

FetchContent_Declare(
  metacg
  GIT_REPOSITORY https://github.com/TimHeldmann/MetaCG.git
  GIT_TAG        f9607447c619c9fe3cd4075fb9ddb053aa1120ba
)

FetchContent_MakeAvailable(metacg)

find_package(
        LLVM
        REQUIRED
        CONFIG
)

set(METAVIRT_LOG_LEVEL
        "0"
        CACHE STRING "MetaVirt log level from 0 (least verbose) to 4 (most verbose)"
)
FetchContent_Declare(
        metavirt
        GIT_REPOSITORY https://github.com/ahueck/llvm-metavirt.git
        GIT_TAG devel
        GIT_SHALLOW 1
        FIND_PACKAGE_ARGS
)
FetchContent_MakeAvailable(metavirt)

add_library(acg-plugin SHARED src/ACGPlugin.cpp)

target_include_directories(acg-plugin PRIVATE include ${LLVM_INCLUDE_DIRS})
target_include_directories(acg-plugin PRIVATE $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>)
target_link_libraries(acg-plugin PRIVATE metacg::metacg)
target_link_libraries(acg-plugin PRIVATE metavirt::Types)
