cmake_minimum_required(VERSION 3.20)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17")

project(example)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

include(libigl)
igl_include(opengl)
igl_include(glfw)
igl_include(imgui)
igl_include(png)

option(LIBIGL_IMGUI "Use Imgui" ON)
option(LIBIGL_PNG "Use PNG" ON)

find_package(PkgConfig REQUIRED)
pkg_check_modules(PORTAUDIO2 REQUIRED portaudio-2.0)
set(PORTAUDIO_LIBRARIES ${PORTAUDIO2_LIBRARY_DIRS}/libportaudio.dylib)

find_package(Protobuf REQUIRED CONFIG)
set(Protobuf_LIBRARIES protobuf::libprotobuf)

# Project files
file(GLOB SRCFILES *.cpp *.cc)
file(GLOB TOOLS tools/*.cpp)
include_directories(${CMAKE_CURRENT_SOURCE_DIR})

foreach(TOOL_NAME ${TOOLS})
    get_filename_component(TOOL_BIN ${TOOL_NAME} NAME_WE)
    set(TOOL_BIN ${TOOL_BIN}_bin)
    message("-- Building tool : ${TOOL_BIN}")
    add_executable(${TOOL_BIN} ${SRCFILES} ${TOOL_NAME})
    target_link_libraries(${TOOL_BIN} ${PORTAUDIO_LIBRARIES} igl::core igl::imgui igl::png ${Protobuf_LIBRARIES})
endforeach()
