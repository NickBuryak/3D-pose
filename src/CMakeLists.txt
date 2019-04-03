﻿cmake_minimum_required(VERSION 3.9)
project(OpticalMocap LANGUAGES CXX)

# Find Qt package and set MOC and RCC vars
set(CMAKE_INCLUDE_CURRENT_DIR ON)

# Include OpenMP
# Include OptiTrack headers, libraries
set(OptiTrack_INCLUDE_DIR "$ENV{OptiTrack_DIR}/include")
include_directories(${OptiTrack_INCLUDE_DIR})
set(OptiTrack_LIB_DIR "$ENV{OptiTrack_DIR}/lib")
add_library(OptiTrack::CameraLibrary2015x64S STATIC IMPORTED)
set_property(TARGET OptiTrack::CameraLibrary2015x64S PROPERTY IMPORTED_LOCATION ${OptiTrack_LIB_DIR}/CameraLibrary2015x64S.lib)

#Include OpenCV package, headers, libraries
find_package(OpenCV REQUIRED)
include_directories(${OpenCV_INCLUDE_DIRS})
link_directories(${OpenCV_LIB_DIR})

#Include Boost package, headers, libraries
find_package(Boost REQUIRED)
include_directories(${Boost_INCLUDE_DIRS})
link_directories(${Boost_LIBRARY_DIRS})

# Add source to this project's executable.
add_executable (${PROJECT_NAME} "Main.cpp" 
								"view/Console.cpp" "view/Console.h"
								"model/scene/SceneManager.cpp" "model/scene/SceneManager.h"
								"model/camera/OptitrackCamera.cpp" "model/camera/OptitrackCamera.h"
								"model/scene/enum/CaptureMode.h")

# Link it all together
target_link_libraries(${PROJECT_NAME} ${OpenCV_LIBS} Boost::boost OptiTrack::CameraLibrary2015x64S)