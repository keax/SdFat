cmake_minimum_required(VERSION 2.6)

###############################################################################
# EMBEDDED
###############################################################################
set(EMBEDDED_PATH ${CMAKE_CURRENT_LIST_DIR}/../embedded/)
include(${EMBEDDED_PATH}/backends/cmake/project.cmake)

###############################################################################
# LIB SDFATLIB
###############################################################################
set(SDFATLIB_SOURCE_PATH ${CMAKE_CURRENT_LIST_DIR}/../SdFat)
file(GLOB SDFATLIB_SOURCE_FILES ${SDFATLIB_SOURCE_PATH}/*)
list(REMOVE_ITEM SDFATLIB_SOURCE_FILES ${SDFATLIB_SOURCE_PATH}/SdSpiSAM3X.cpp ${SDFATLIB_SOURCE_PATH}/StdioStream.cpp)
build_library(sdfatlib ${SDFATLIB_SOURCE_FILES})
target_include_directories(sdfatlib PUBLIC ${SDFATLIB_SOURCE_PATH})
target_compile_options(sdfatlib PUBLIC -includetypes.hpp)
target_compile_options(sdfatlib PUBLIC -includefrontend_arduino.hpp)
#target_compile_options(sdfatlib PRIVATE -nostdinc)
target_link_libraries(sdfatlib pinout frontend_arduino)
target_compile_definitions(sdfatlib PUBLIC -DUSE_NATIVE_SAM3X_SPI)

###############################################################################
# PROJECT
###############################################################################
set(PROJECT_SOURCES main.cpp)

build_project(sdlogger ${PROJECT_SOURCES})
target_link_libraries(sdlogger sdfatlib)


