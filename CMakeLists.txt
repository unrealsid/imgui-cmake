cmake_minimum_required(VERSION 4.0.0 FATAL_ERROR)

project(imgui LANGUAGES C CXX)

file(GLOB SRC_FILES CONFIGURE_DEPENDS
    "${CMAKE_CURRENT_SOURCE_DIR}/*.cpp"
    "${CMAKE_CURRENT_SOURCE_DIR}/*.h"
    "${CMAKE_CURRENT_SOURCE_DIR}/*.c"
)

set(HEADER_FILES
    "${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_sdl3.h"
    "${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_vulkan.h"
)

set(SOURCE_FILES
    "${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_vulkan.cpp"
)

set(ALL_FILES
    ${SRC_FILES}
    ${HEADER_FILES}
    ${SOURCE_FILES}
)

add_library(imgui STATIC ${ALL_FILES})

find_package(Vulkan REQUIRED)
find_package(SDL3-shared REQUIRED)

target_include_directories(imgui PUBLIC
    "${CMAKE_CURRENT_SOURCE_DIR}"
    "${CMAKE_CURRENT_SOURCE_DIR}/backends"
)

target_link_libraries(imgui PUBLIC
    Vulkan::Vulkan
    SDL3-shared::SDL3-shared
)

add_library(imgui::imgui ALIAS imgui)
