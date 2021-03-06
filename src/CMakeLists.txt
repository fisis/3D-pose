﻿cmake_minimum_required(VERSION 3.9)
project(3DPose LANGUAGES CXX)

# Find Qt package and set MOC and RCC vars
set(CMAKE_INCLUDE_CURRENT_DIR ON)

# Include OpenMP
find_package(OpenMP REQUIRED)
if (OPENMP_FOUND)
    set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
    set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
    set (CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}")
endif()

# Include OptiTrack
set(OptiTrack_INCLUDE_DIR "$ENV{OptiTrack_DIR}/include")
include_directories(${OptiTrack_INCLUDE_DIR})
set(OptiTrack_LIB_DIR "$ENV{OptiTrack_DIR}/lib")
add_library(OptiTrack::CameraLibrary2015x64S STATIC IMPORTED)
set_property(TARGET OptiTrack::CameraLibrary2015x64S PROPERTY IMPORTED_LOCATION ${OptiTrack_LIB_DIR}/CameraLibrary2015x64S.lib)

#Include OpenCV
find_package(OpenCV REQUIRED)
include_directories(${OpenCV_INCLUDE_DIRS})
link_directories(${OpenCV_LIB_DIR})

#Include Boost
find_package(Boost REQUIRED)
include_directories(${Boost_INCLUDE_DIRS})
link_directories(${Boost_LIBRARY_DIRS})

#Include VTK
find_package(VTK REQUIRED)
include(${VTK_USE_FILE})

# Add source to this project's executable.
add_executable (${PROJECT_NAME} "Main.cpp" 
								"view/enum/Input.h"
								"view/Console.cpp" "view/Console.h"
								"view/rendering/Renderer2D.cpp" "view/rendering/Renderer2D.h"
								"view/rendering/Renderer3D.cpp" "view/rendering/Renderer3D.h"
								"model/AppController.cpp" "model/AppController.h"
								"model/scene/Scene.cpp" "model/scene/Scene.h"
								"model/scene/SceneController.cpp" "model/scene/SceneController.h"
								"model/capture/CaptureType.h"
								"model/capture/Capture.cpp" "model/capture/Capture.h"	
								"model/video/Packet3D.cpp" "model/video/Packet3D.h"	
								"model/video/Frame3D.cpp" "model/video/Frame3D.h"	
								"model/video/Video3D.cpp" "model/video/Video3D.h"	
								"model/camera/Packet.cpp" "model/camera/Packet.h"
								"model/camera/optitrack/OptitrackCamera.cpp" "model/camera/optitrack/OptitrackCamera.h"
								"model/camera/CameraController.cpp" "model/camera/CameraController.h"
								"model/config/ConfigController.cpp" "model/config/ConfigController.h"
								"model/calibration/CalibrationType.h"
 								"model/calibration/Intrinsics.cpp" "model/calibration/Intrinsics.h"
								"model/calibration/Extrinsics.cpp" "model/calibration/Extrinsics.h"								
								"model/calibration/CalibrationController.cpp" "model/calibration/CalibrationController.h")

# Link it all together
target_link_libraries(${PROJECT_NAME} ${OpenCV_LIBS} ${VTK_LIBRARIES} Boost::boost OptiTrack::CameraLibrary2015x64S)