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
file(GLOB SRC_FILES
        "${CMAKE_CURRENT_SOURCE_DIR}/*.cpp"
        "${CMAKE_CURRENT_SOURCE_DIR}/*.h"
        "${CMAKE_CURRENT_SOURCE_DIR}/*.c"
)

#For 'src/backends' directory, only need SDL3 and Vulkan specific parts. 
#Will need to change this if using GLFW or any other window rendering framework
set(
    Header_Files
    "${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_sdl3.h"
    "${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_vulkan.h"
)

set(
    Source_Files
    "${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_vulkan.cpp"
)

set(
    ALL_FILES
    ${Header_Files}
    ${Source_Files}
    ${SRC_FILES}
)

add_library(
    ${PROJECT_NAME} 
    STATIC
	${SRC_FILES}
)

find_package(Vulkan REQUIRED)

target_include_directories(
    ${PROJECT_NAME} 
    PUBLIC
    "${CMAKE_CURRENT_SOURCE_DIR}"
)

target_link_libraries(${PROJECT_NAME} PUBLIC Vulkan::Vulkan
                                             SDL3-shared)

add_library(${PROJECT_NAME}::${PROJECT_NAME} ALIAS ${PROJECT_NAME})
