cmake_minimum_required( VERSION 3.0 )

project( vulkan-project )

set( CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17 -O2" )

set( CMAKE_EXPORT_COMPILE_COMMANDS ON )

include_directories( ./include )

if( APPLE )
	include_directories( /usr/local/include )
	link_directories( /usr/local/lib )
endif()


file( GLOB SOURCES "./src/*.cpp" "./src/**/*.cpp" )

file( GLOB TEST_SOURCES "./src/**/*.cpp" "./test/*.cpp" "./test/**/*.cpp" )

add_executable( vulkan-game ${SOURCES} )

add_executable( vulkan-tests ${TEST_SOURCES} )

target_link_libraries( vulkan-game glfw vulkan dl pthread )

if( UNIX AND NOT APPLE )
	target_link_libraries( vulkan-game X11 Xxf86vm Xrandr Xi )
endif()
