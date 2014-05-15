cmake_minimum_required(VERSION 2.8 FATAL_ERROR)

project(PCL_KinectKinfu)

SET_PROPERTY(GLOBAL PROPERTY TARGET_SUPPORTS_SHARED_LIBS TRUE)

IF(WIN32 OR win64)
   SET(CMAKE_FIND_LIBRARY_SUFFIXES .lib .dll)
ELSE()
   SET(CMAKE_FIND_LIBRARY_SUFFIXES .a)
ENDIF()

set(OPENNI_LIBRARY "C:/Program Files/OpenNI/Lib64/OpenNI64.lib")
set(OpenCV_DIR "C:/Users/ur10/Downloads/opencv/build")
set(PCL_DIR "C:/Users/ur10/Downloads/Repositories/pcl-trunk/build64")

find_package(PCL 1.7 REQUIRED)
find_package( OpenCV REQUIRED )
FIND_PACKAGE(CUDA REQUIRED)
 
SET(CUDA_NVCC_FLAGS "-arch=sm_20" CACHE STRING "nvcc flags" FORCE)
#SET (CUDA_VERBOSE_BUILD ON CACHE BOOL "nvcc verbose" FORCE)

add_subdirectory(src)


set(KinectSDK_INCLUDE "C:/Program Files/Microsoft SDKs/Kinect/v2.0-DevPreview1404/inc" CACHE PATH "Include directory for Kinect2 SDK")
set(KinectSDK_LIB "C:/Program Files/Microsoft SDKs/Kinect/v2.0-DevPreview1404/lib/x64" CACHE PATH "Library directory for Kinect2 SDK")

set(PCL_Kinect2SDK_INCLUDE "../PCL_Kinect2SDK/include" CACHE PATH "Include directory for PCL_Kinect2SDK project")
set(PCL_Kinect2SDK_LIBDIR "C:/Users/steph_000/Documents/GitHub/BUILDS/PCL_Kinect2SDK" CACHE PATH "Library directory for PCL_Kinect2SDK project")

include_directories(${OpenCV_INCLUDE_DIRS})
include_directories(${PCL_INCLUDE_DIRS})
include_directories(${PCL_KinectKinfu_SOURCE_DIR}/include)
include_directories(${PCL_Kinect2SDK_INCLUDE})
include_directories(${CUDA_TOOLKIT_INCLUDE})
include_directories(${KinectSDK_INCLUDE})

link_directories(${PCL_LIBRARY_DIRS})
link_directories(${PCL_Kinect2SDK_LIBDIR})
link_directories(${KinectSDK_LIB})

add_definitions(${PCL_DEFINITIONS})

file(GLOB_RECURSE PCL_KinectKinfu_HEADERS include/*.h)
file(GLOB_RECURSE PCL_KinectKinfu_SOURCES src/*.cpp)
set (PCL_KinectKinfu_INCLUDE_DIRS "")
foreach (_headerFile ${PCL_KinectKinfu_HEADERS})
    get_filename_component(_dir ${_headerFile} PATH)
    list (APPEND PCL_KinectKinfu_INCLUDE_DIRS ${_dir})
endforeach()
list(REMOVE_DUPLICATES PCL_KinectKinfu_INCLUDE_DIRS)

include_directories(${PCL_KinectKinfu_INCLUDE_DIRS})

add_executable (PCL_KinectKinfu ${PCL_KinectKinfu_SOURCES} ${PCL_KinectKinfu_HEADERS})
target_link_libraries (PCL_KinectKinfu ${CUDA_CUDA_LIBRARY} ${CUDA_CUDART_LIBRARY} ${PCL_LIBRARIES} ${OpenCV_LIBS} Kinect20.lib debug Debug/PCL_Kinect2SDK optimized Release/PCL_Kinect2SDK)
		
add_custom_command(TARGET PCL_KinectKinfu POST_BUILD        		# Adds a post-build event to MyTest
	COMMAND ${CMAKE_COMMAND} -E copy_if_different  			# which executes "cmake - E copy_if_different..."
		"${_OpenCV_LIB_PATH}/opencv_calib3d248d.dll"      	# <--this is in-file
		$<TARGET_FILE_DIR:PCL_KinectKinfu>)                 		# <--this is out-file path
		
add_custom_command(TARGET PCL_KinectKinfu POST_BUILD        		# Adds a post-build event to MyTest
	COMMAND ${CMAKE_COMMAND} -E copy_if_different  			# which executes "cmake - E copy_if_different..."
		"${_OpenCV_LIB_PATH}/opencv_contrib248d.dll"      	# <--this is in-file
		$<TARGET_FILE_DIR:PCL_KinectKinfu>)                 		# <--this is out-file path
		
add_custom_command(TARGET PCL_KinectKinfu POST_BUILD        		# Adds a post-build event to MyTest
	COMMAND ${CMAKE_COMMAND} -E copy_if_different  			# which executes "cmake - E copy_if_different..."
		"${_OpenCV_LIB_PATH}/opencv_core248d.dll"      	# <--this is in-file
		$<TARGET_FILE_DIR:PCL_KinectKinfu>)  # <--this is out-file path
		
add_custom_command(TARGET PCL_KinectKinfu POST_BUILD
	COMMAND ${CMAKE_COMMAND} -E copy_if_different
		"${_OpenCV_LIB_PATH}/opencv_features2d248d.dll"
		$<TARGET_FILE_DIR:PCL_KinectKinfu>)
		
add_custom_command(TARGET PCL_KinectKinfu POST_BUILD
	COMMAND ${CMAKE_COMMAND} -E copy_if_different
		"${_OpenCV_LIB_PATH}/opencv_flann248d.dll"
		$<TARGET_FILE_DIR:PCL_KinectKinfu>)
		
add_custom_command(TARGET PCL_KinectKinfu POST_BUILD
	COMMAND ${CMAKE_COMMAND} -E copy_if_different
		"${_OpenCV_LIB_PATH}/opencv_highgui248d.dll"
		$<TARGET_FILE_DIR:PCL_KinectKinfu>)
		
add_custom_command(TARGET PCL_KinectKinfu POST_BUILD
	COMMAND ${CMAKE_COMMAND} -E copy_if_different
		"${_OpenCV_LIB_PATH}/opencv_imgproc248d.dll"
		$<TARGET_FILE_DIR:PCL_KinectKinfu>)
		
add_custom_command(TARGET PCL_KinectKinfu POST_BUILD
	COMMAND ${CMAKE_COMMAND} -E copy_if_different
		"${_OpenCV_LIB_PATH}/opencv_ml248d.dll"
		$<TARGET_FILE_DIR:PCL_KinectKinfu>)
		
add_custom_command(TARGET PCL_KinectKinfu POST_BUILD
	COMMAND ${CMAKE_COMMAND} -E copy_if_different
		"${_OpenCV_LIB_PATH}/opencv_objdetect248d.dll"
		$<TARGET_FILE_DIR:PCL_KinectKinfu>)
		
add_custom_command(TARGET PCL_KinectKinfu POST_BUILD
	COMMAND ${CMAKE_COMMAND} -E copy_if_different
		"${_OpenCV_LIB_PATH}/opencv_video248d.dll"
		$<TARGET_FILE_DIR:PCL_KinectKinfu>)