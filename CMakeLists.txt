#[[
Copyright (C) 2026 Marco M. Mosca

This file is part of periodic-point-cloud.

periodic-point-cloud is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

periodic-point-cloud is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with periodic-point-cloud. If not, see <https://www.gnu.org/licenses/>.
*]]
cmake_minimum_required (VERSION 3.16)

project (
    periodic_point_cloud
    VERSION 0.1.0
    DESCRIPTION  "Periodic point cloud generator"
    LANGUAGES CXX
)

include(FetchContent)

FetchContent_Declare(
    Eigen3
    URL https://gitlab.com/libeigen/eigen/-/archive/3.4.0/eigen-3.4.0.tar.gz
)

set(BUILD_TESTING OFF CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(Eigen3)
set(BUILD_TESTING ON CACHE BOOL "" FORCE)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

add_library(periodic_point_cloud
    src/periodicpointcloud.cpp
)

target_include_directories(periodic_point_cloud
    PUBLIC
        $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
)

target_link_libraries(periodic_point_cloud
    PRIVATE
        Eigen3::Eigen
)

if (BUILD_TESTING)
    enable_testing()
    include(GoogleTest)
    add_subdirectory ("tests")
endif()

install(
    TARGETS periodic_point_cloud
    EXPORT periodic_point_cloudTargets
)

install(
    DIRECTORY include/
    DESTINATION include
)

install(
    EXPORT periodic_point_cloudTargets
    FILE periodic_point_cloudTargets.cmake
    NAMESPACE periodic_point_cloud::
    DESTINATION lib/cmake/periodic_point_cloud
)
