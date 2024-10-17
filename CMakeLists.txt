cmake_minimum_required(VERSION 3.16)
set(CMAKE_EXPORT_COMPILE_COMMANDS 1)
project(your_project_name)

set(CMAKE_CXX_STANDARD 17)

option(NIXOS "Dont try to compile GLFW on nixos, just link it" OFF)

if(NIXOS)
link_libraries(GL glfw)
else()
set(GLFW_BUILD_DOCS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_TESTS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)

add_subdirectory(dependencies/glfw)
endif()

include_directories(
   include
   dependencies/glad
)

FILE(GLOB SRC_FILES src/**.c src/**.cc src/**.cpp)
#to add source files manually:
#set(SRC_FILES
#   src/main.c
#)

add_library(glad dependencies/glad/glad.c)

add_executable(${CMAKE_PROJECT_NAME} src/main.c)
target_link_libraries(${CMAKE_PROJECT_NAME} glad glfw)
target_sources(${CMAKE_PROJECT_NAME} PRIVATE ${SRC_FILES})