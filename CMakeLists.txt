cmake_minimum_required( VERSION 3.6 )

project(imgui-cmake)

if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
  # Set the policy to prefer GLVND only on Linux
  cmake_policy(SET CMP0072 NEW)
endif()

find_package( OpenGL REQUIRED )

include_directories( ${OPENGL_INCLUDE_DIRS} )

set( GLFW_BUILD_DOCS OFF CACHE BOOL  "GLFW lib only" )
set( GLFW_INSTALL OFF CACHE BOOL  "GLFW lib only" )

add_subdirectory( libs/glfw )

add_library(imgui 
    libs/imgui/imgui.cpp
    libs/imgui/imgui.h
    libs/imgui/imgui_demo.cpp
    libs/imgui/imgui_draw.cpp
    libs/imgui/imgui_tables.cpp
    libs/imgui/imgui_widgets.cpp
    libs/imgui/backends/imgui_impl_glfw.cpp
    libs/imgui/backends/imgui_impl_opengl3.cpp
    libs/imgui/imconfig.h
    libs/imgui/imgui_internal.h
    libs/imgui/backends/imgui_impl_glfw.h
    libs/imgui/backends/imgui_impl_opengl3.h
    libs/imgui/backends/imgui_impl_opengl3_loader.h
)

target_include_directories(imgui PUBLIC ${CMAKE_SOURCE_DIR}/libs/imgui ${CMAKE_SOURCE_DIR}/libs/imgui/backends ${CMAKE_SOURCE_DIR}/libs/glfw/include)

if(UNIX AND NOT APPLE)
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -static-libgcc -static-libstdc++")
else()
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -static-libgcc -static-libstdc++ -static -lpthread")
endif()

add_compile_options(-Wall -Wformat)
add_executable(imgui-cmake src/main.cpp)
# Use WIN32 on Windows if you don't want to see terminal window.

target_link_libraries( imgui-cmake ${OPENGL_LIBRARIES} glfw )
target_link_libraries(imgui-cmake imgui)