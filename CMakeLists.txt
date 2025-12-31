cmake_minimum_required(
	VERSION 4.0.0 
	FATAL_ERROR
)

project(
  imgui
	LANGUAGES C
)

include(GNUInstallDirs)

# Automatically get all .cpp, .h and.c files from the src directory
file(GLOB_RECURSE SRC_FILES
        "${CMAKE_CURRENT_SOURCE_DIR}/*.cpp"
        "${CMAKE_CURRENT_SOURCE_DIR}/*.h"
        "${CMAKE_CURRENT_SOURCE_DIR}/*.c"
)

add_library(
    ${PROJECT_NAME} 
    STATIC
	${SRC_FILES}
)

target_include_directories(
    ${PROJECT_NAME} 
    PUBLIC
    "${CMAKE_CURRENT_SOURCE_DIR}"
)

add_library(${PROJECT_NAME}::${PROJECT_NAME} ALIAS ${PROJECT_NAME})
