#1.cmake版本
cmake_minimum_required(VERSION 2.8)

#2.指定工程
project(vtk_pcd_3d)


set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
# set(CMAKE_CUDA_STANDARD 14)
# set(CMAKE_CUDA_STANDARD_REQUIRED ON)

#3.针对Qt的自动预处理
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTORCC ON)

#4.跨平台
# IF(CMAKE_SYSTEM_NAME MATCHES "Windows")
#     set(CMAKE_C_COMPILER "clang")
#      link_libraries(ws2_32 wsock32)             #使用windows下的套接字需要此行代码
# ELSEIF(CMAKE_SYSTEM_NAME MATCHES "Linux")
#     set(CMAKE_C_COMPILER "clang") 				#或gcc
#     set(CMAKE_CXX_COMPILER "clang++") 			#或g++
#     set(CMAKE_CXX_FLAGS "-std=c++11 -O3 -Wall") #c++11标准、O3编译优化、显示所有（warning all）编译过程警告
# ENDIF()

#5.编译模式
set(CMAKE_BUILD_TYPE "Debug")

#6.找到包

find_package(OpenGL REQUIRED)
if(OPENGL_FOUND)
    message(STATUS "OPENGL_INCLUDE_DIRS: ${OPENGL_INCLUDE_DIRS}")
    message(STATUS "OPENGL_LIBRARIES: ${OPENGL_LIBRARIES}")
else()
    message (FATAL_ERROR "Cannot find OpenGL")
endif()


find_package(VTK REQUIRED)
if(VTK_FOUND)
    message(STATUS "VTK_INCLUDE_DIRS: ${VTK_INCLUDE_DIRS}")
    message(STATUS "VTK_LIBRARY_DIRS: ${VTK_LIBRARY_DIRS}")
    message(STATUS "VTK_LIBRARIES: ${VTK_LIBRARIES}")
else()
    message (FATAL_ERROR "Cannot find VTK")
endif()

# PCL
find_package(PCL REQUIRED)
if(PCL_FOUND)
    message(STATUS "PCL_INCLUDE_DIRS: ${PCL_INCLUDE_DIRS}")
    message(STATUS "PCL_LIBRARY_DIRS: ${PCL_LIBRARY_DIRS}")
    message(STATUS "PCL_VERSION: ${PCL_VERSION}")
else()
    message (FATAL_ERROR "Cannot find pcl")
endif()
add_definitions(${PCL_DEFINITIONS})
# Qt5
find_package(Qt6 COMPONENTS Widgets Core Gui Network REQUIRED)
# OpenCV
# find_package( OpenCV 4.3.0 REQUIRED)
# Eigen3
find_package( Eigen3 REQUIRED )
# G2O
# find_package( G2O REQUIRED )
# Pangoling
#find_package( Pangolin REQUIRED)
# Sophus
#find_package( Sophus REQUIRED)
# Boost
find_package( Boost REQUIRED)
if(Boost_FOUND)
    message(STATUS "Boost_INCLUDE_DIRS: ${Boost_INCLUDE_DIRS}")
    message(STATUS "Boost_LIBRARY_DIRS: ${Boost_LIBRARY_DIRS}")
else(Boost_FOUND)
    message(FATAL_ERROR ”Boost library not found”)
endif(Boost_FOUND)

#7.头文件
# include(${VTK_USE_FILE}) #针对VTK
# set(CMAKE_INCLUDE_CURRENT_DIR ON) #相当于
# INCLUDE_DIRECTORIES(${CMAKE_CURRENT_BINARY_DIR} ${CMAKE_CURRENT_SOURCE_DIR})

set(SUBSUBSYS_NAME vtk_pcd_editor)
# set(SUBSUBSYS_DESC "Point Cloud Editor - Simple editor for 3D point clouds")
# set(SUBSUBSYS_DEPS common filters io apps)
# set(SUBSUBSYS_EXT_DEPS vtk ${QTX} OpenGL OpenGL_GLU)
# set(REASON "")
# set(DEFAULT OFF)

# PCL_SUBSUBSYS_OPTION(build "${SUBSYS_NAME}" "${SUBSUBSYS_NAME}" "${SUBSUBSYS_DESC}" ${DEFAULT} "${REASON}")
# PCL_SUBSYS_DEPEND(build NAME ${SUBSUBSYS_NAME} PARENT_NAME ${SUBSYS_NAME} DEPS ${SUBSUBSYS_DEPS} EXT_DEPS ${SUBSUBSYS_EXT_DEPS})
# PCL_ADD_DOC(${SUBSUBSYS_NAME})

# if(NOT build)
#   return()
# endif()

set(RSRC
    resources/pceditor_resources.qrc
)

set(INCS
  "include/pcl/apps/${SUBSUBSYS_NAME}/cloudEditorWidget.h"
  "include/pcl/apps/${SUBSUBSYS_NAME}/mainWindow.h"
  "include/pcl/apps/${SUBSUBSYS_NAME}/denoiseParameterForm.h"
  "include/pcl/apps/${SUBSUBSYS_NAME}/statisticsDialog.h"
  "include/pcl/apps/${SUBSUBSYS_NAME}/cloud.h"
  "include/pcl/apps/${SUBSUBSYS_NAME}/cloudTransformTool.h"
  "include/pcl/apps/${SUBSUBSYS_NAME}/command.h"
  "include/pcl/apps/${SUBSUBSYS_NAME}/commandQueue.h"
  "include/pcl/apps/${SUBSUBSYS_NAME}/common.h"
  "include/pcl/apps/${SUBSUBSYS_NAME}/copyBuffer.h"
  "include/pcl/apps/${SUBSUBSYS_NAME}/copyCommand.h"
  "include/pcl/apps/${SUBSUBSYS_NAME}/cutCommand.h"
  "include/pcl/apps/${SUBSUBSYS_NAME}/deleteCommand.h"
  "include/pcl/apps/${SUBSUBSYS_NAME}/denoiseCommand.h"
  "include/pcl/apps/${SUBSUBSYS_NAME}/localTypes.h"
  "include/pcl/apps/${SUBSUBSYS_NAME}/pasteCommand.h"
  "include/pcl/apps/${SUBSUBSYS_NAME}/select1DTool.h"
  "include/pcl/apps/${SUBSUBSYS_NAME}/select2DTool.h"
  "include/pcl/apps/${SUBSUBSYS_NAME}/selection.h"
  "include/pcl/apps/${SUBSUBSYS_NAME}/selectionTransformTool.h"
  "include/pcl/apps/${SUBSUBSYS_NAME}/statistics.h"
  "include/pcl/apps/${SUBSUBSYS_NAME}/toolInterface.h"
  "include/pcl/apps/${SUBSUBSYS_NAME}/trackball.h"
  "include/pcl/apps/${SUBSUBSYS_NAME}/transformCommand.h"
)

set(SRCS
  src/main.cpp
  src/mainWindow.cpp
  src/commandQueue.cpp
  src/selection.cpp
  src/copyBuffer.cpp
  src/deleteCommand.cpp
  src/cutCommand.cpp
  src/pasteCommand.cpp
  src/cloud.cpp
  src/cloudEditorWidget.cpp
  src/cloudTransformTool.cpp
  src/select1DTool.cpp
  src/select2DTool.cpp
  src/selectionTransformTool.cpp
  src/transformCommand.cpp
  src/common.cpp
  src/denoiseCommand.cpp
  src/statistics.cpp
  src/statisticsDialog.cpp
  src/trackball.cpp
  src/denoiseParameterForm.cpp
)

set(EXE_NAME "pcl_${SUBSUBSYS_NAME}")

add_executable(${EXE_NAME}
    ${SRCS}
    ${RSRC}
    ${INC}
)

#PCL_ADD_EXECUTABLE(
#  ${EXE_NAME}
#  COMPONENT
#    ${SUBSUBSYS_NAME}
#  SOURCES
#    ${SRCS}
#    ${RSRC}
#    ${INCS})

#8.链接库
target_link_libraries("${EXE_NAME}" 
	Qt6::Core
	Qt6::Gui
    Qt6::Widgets 
    Qt6::OpenGL 
    ${OPENGL_LIBRARIES}
    ${BOOST_LIBRARIES}
	${VTK_LIBRARIES}
    ${PCL_LIBRARIES}
)

target_include_directories(${EXE_NAME} 
    PRIVATE 
		${CMAKE_CURRENT_SOURCE_DIR}/include
        ${OPENGL_INCLUDE_DIRS}
		${VTK_LIBRARY_DIRS}
		${PCL_INCLUDE_DIRS}
		${BOOST_INCLUDE_DIRS}
		${EIGEN3_INCLUDE_DIRS}
)


if (${QTX} MATCHES "Qt6")
  target_link_libraries("${EXE_NAME}" ${QTX}::OpenGLWidgets)
endif()

# PCL_ADD_INCLUDES("${SUBSUBSYS_NAME}" "${SUBSYS_NAME}/${SUBSUBSYS_NAME}" ${INCS})
# PCL_MAKE_PKGCONFIG(${EXE_NAME} COMPONENT ${SUBSUBSYS_NAME} DESC ${SUBSUBSYS_DESC})

# Add to the compound apps target
# list(APPEND PCL_APPS_ALL_TARGETS ${EXE_NAME})
# set(PCL_APPS_ALL_TARGETS ${PCL_APPS_ALL_TARGETS} PARENT_SCOPE)
