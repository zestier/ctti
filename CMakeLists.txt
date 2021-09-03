project(ctti VERSION 0.0.2)
cmake_minimum_required(VERSION 3.14)

option(CTTI_BUILD_TESTS "Build ctti feature tests" OFF)
option(CTTI_BUILD_EXAMPLES "Build ctti examples" OFF)

include(cmake/vs_source_groups.cmake)

set(CTTI_SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR})

set(CMAKE_CXX_STANDARD 14)
if(NOT MSVC)
    set(CMAKE_CXX_FLAGS "-Wall -Wextra -Wall -Werror")
endif()

add_library(ctti INTERFACE)
target_include_directories(ctti INTERFACE ${CTTI_SOURCE_DIR}/include)

if(CTTI_BUILD_TESTS)
    enable_testing()
    add_subdirectory(tests)
endif()

if(CTTI_BUILD_EXAMPLES)
    add_subdirectory(examples)
endif()


# Package the project for installation
include(cmake/CPM.cmake)
CPMAddPackage("gh:TheLartians/PackageProject.cmake@1.6.0")
packageProject(
  NAME ${PROJECT_NAME}
  VERSION ${PROJECT_VERSION}
  BINARY_DIR ${PROJECT_BINARY_DIR}
  INCLUDE_DIR ${PROJECT_SOURCE_DIR}/inc
  INCLUDE_DESTINATION inc/${PROJECT_NAME}-${PROJECT_VERSION}
  VERSION_HEADER "${PROJECT_NAME}/version.h"
  EXPORT_HEADER "${PROJECT_NAME}/export.h"
  COMPATIBILITY SameMajorVersion
  DISABLE_VERSION_SUFFIX YES
)