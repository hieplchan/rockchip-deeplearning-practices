cmake_minimum_required(VERSION 3.10)
project(gstrkfacedetect)


find_package(OpenCV REQUIRED)

find_package(PkgConfig)
pkg_check_modules(GSTREAMER REQUIRED gstreamer-1.0 gstreamer-base-1.0 glib-2.0)

add_definitions(-DPACKAGE="RK3399")
add_definitions(-DPACKAGE_VERSION="1.0.0")
add_definitions(-DGST_PACKAGE_NAME="Face Detection")
add_definitions(-DGST_PACKAGE_ORIGIN="hiep.lchan@gmail.com")
add_definitions(-DGST_LICENSE="LGPL")

file(GLOB SOURCES RELATIVE ${CMAKE_CURRENT_SOURCE_DIR}
    src/*
    inc/*)

# MNN
set(MNN_LIBRARY_DIRS ${CMAKE_CURRENT_SOURCE_DIR}/../3rdparty/mnn/lib)
set(MNN_INCLUDE_DIRS ${CMAKE_CURRENT_SOURCE_DIR}/../3rdparty/mnn/include)

include_directories(
        ${OpenCV_INCLUDE_DIRS}
        ${GLIB_INCLUDE_DIRS}
        ${GSTREAMER_INCLUDE_DIRS}
        ${MNN_INCLUDE_DIRS}
        ${CMAKE_CURRENT_SOURCE_DIR}/inc
        ${CMAKE_CURRENT_SOURCE_DIR}/../gst-libs 
)

link_directories(
        /usr/lib/aarch64-linux-gnu/gstreamer-1.0
        ${GLIB_LIBRARY_DIRS}
        ${GSTREAMER_LIBRARY_DIRS}
        ${GSTREAMER_BASE_LIBRARY_DIRS}
        ${MNN_LIBRARY_DIRS}
)

add_library(${PROJECT_NAME} SHARED ${SOURCES})

target_link_libraries(${PROJECT_NAME}
        MNN
        MNN_CL
        gstinferdatameta
        ${OpenCV_LIBS}
        ${GLIB2_LIBRARIES}
        ${GSTREAMER_LIBRARIES}
        ${GSTREAMER_BASE_LIBRARIES}
)