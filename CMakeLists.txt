cmake_minimum_required(VERSION 3.10)

if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release)
endif()

project(libOpenDrive VERSION 0.3.0 DESCRIPTION ".xodr library")
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED True)

if(MSVC)
    set(CMAKE_CXX_FLAGS "/EHsc /Wall")
    set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS 1)
    set(CMAKE_CXX_FLAGS_RELEASE "/O2")
else()
    set(CMAKE_CXX_FLAGS "-Wall")
    set(CMAKE_CXX_FLAGS_DEBUG "-g")
    set(CMAKE_CXX_FLAGS_RELEASE "-O3")
endif()

find_package(pugixml REQUIRED)

set(SOURCES 
    src/Geometries/Arc.cpp
    src/Geometries/CubicSpline.cpp
    src/Geometries/Line.cpp
    src/Geometries/ParamPoly3.cpp
    src/Geometries/RoadGeometry.cpp
    src/Geometries/Spiral.cpp
    src/Geometries/Spiral/odrSpiral.cpp
    src/Junction.cpp
    src/Lane.cpp
    src/LaneSection.cpp
    src/Mesh.cpp
    src/OpenDriveMap.cpp
    src/RefLine.cpp
    src/Road.cpp
    src/RoadMark.cpp
    src/RoadNetworkMesh.cpp
    src/RoadObject.cpp
    src/RoadSignal.cpp
    src/RoutingGraph.cpp
)

add_library(OpenDrive ${SOURCES})

target_link_libraries(OpenDrive PUBLIC pugixml)

target_include_directories(OpenDrive
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include> $<INSTALL_INTERFACE:include>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/thirdparty>)

add_executable(test-xodr test.cpp)
target_link_libraries(test-xodr OpenDrive)

install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/ DESTINATION "include")

install(TARGETS OpenDrive EXPORT OpenDriveConfig)
install(EXPORT OpenDriveConfig NAMESPACE OpenDrive:: DESTINATION cmake)
