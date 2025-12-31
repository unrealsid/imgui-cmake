cmake_minimum_required(VERSION 4.0.0 FATAL_ERROR)

project(imgui LANGUAGES CXX)

include(GNUInstallDirs)

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
    "${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_sdl3.cpp"
    "${CMAKE_CURRENT_SOURCE_DIR}/backends/imgui_impl_vulkan.cpp"
)

set(ALL_FILES
    ${SRC_FILES}
    ${HEADER_FILES}
    ${SOURCE_FILES}
)

add_library(imgui STATIC ${ALL_FILES})

find_package(Vulkan REQUIRED)

target_include_directories(imgui PUBLIC
    "${CMAKE_CURRENT_SOURCE_DIR}"
    "${CMAKE_CURRENT_SOURCE_DIR}/backends"
)

if (TARGET SDL3::SDL3)
    message(STATUS "SDL3 is usable")
else()
    message(STATUS "SDL3 missing or broken")
endif()

target_link_libraries(imgui PUBLIC
    Vulkan::Vulkan
    SDL3::SDL3
)

add_library(imgui::imgui ALIAS imgui)
