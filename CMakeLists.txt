cmake_minimum_required( VERSION 3.0 )

project( vulkan-project )

set( CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17 -O2" )

set( CMAKE_EXPORT_COMPILE_COMMANDS ON )

find_package( Boost 1.71.0 REQUIRED )

include_directories( ./include ${Boost_INCLUDE_DIRS} )

file( GLOB SOURCES "./src/*.cpp" "./src/**/*.cpp" )

file( GLOB TEST_SOURCES "./src/**/*.cpp" "./test/*.cpp" "./test/**/*.cpp" )

add_executable( vulkan-game ${SOURCES} )

add_executable( vulkan-tests ${TEST_SOURCES} )

target_link_libraries( vulkan-game -lglfw -lvulkan -ldl -lpthread -lX11 -lXxf86vm -lXrandr -lXi )
