# Example usage:
#  mkdir build
#  cd build
#  cmake -g "Visual Studio 14 2015" ..

cmake_minimum_required(VERSION 2.9)
project(imgui_example_sdl2_vulkan C CXX)

if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Debug CACHE STRING "" FORCE)
endif()

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DVK_PROTOTYPES")
set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DVK_PROTOTYPES")

# SDL2
find_package(SDL2 REQUIRED)

# Dear ImGui
set(IMGUI_DIR ../)
include_directories(${IMGUI_DIR} ${IMGUI_DIR}/implot ${IMGUI_DIR}/backends ${IMGUI_DIR}/misc/cpp ..)

find_package(GSL REQUIRED)

set(GSL_FIT_DIR ./include/external/gsl-curve-fit/)
include_directories(${GSL_FIT_DIR})

# Libraries
find_package(Vulkan REQUIRED)
#find_library(VULKAN_LIBRARY
  #NAMES vulkan vulkan-1)
set(LIBRARIES "GSL::gsl;GSL::gslcblas;SDL2::SDL2;Vulkan::Vulkan")

# Use vulkan headers from glfw:
#include_directories(${GLFW_DIR}/deps)

file(GLOB sources *.cpp)

add_executable(example_sdl2_vulkan ${sources} ${IMGUI_DIR}/backends/imgui_impl_sdl2.cpp 
${IMGUI_DIR}/backends/imgui_impl_vulkan.cpp ${IMGUI_DIR}/imgui.cpp 
${IMGUI_DIR}/imgui_draw.cpp ${IMGUI_DIR}/imgui_demo.cpp ${IMGUI_DIR}/imgui_tables.cpp 
${IMGUI_DIR}/imgui_widgets.cpp ${IMGUI_DIR}/implot/implot.cpp ${IMGUI_DIR}/implot/implot_items.cpp
${GSL_FIT_DIR}/curve_fit.cpp)
target_link_libraries(example_sdl2_vulkan ${LIBRARIES} )
target_compile_definitions(example_sdl2_vulkan PUBLIC -DImTextureID=ImU64)
