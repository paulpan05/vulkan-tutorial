cmake_minimum_required( VERSION 3.1 )

set( CMAKE_BUILD_TYPE "Release" )
set( CMAKE_CXX_STANDARD 17 )
set( CMAKE_CXX_STANDARD_REQUIRED ON )
set( CMAKE_EXPORT_COMPILE_COMMANDS ON )

if( UNIX )
	set( CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O2" )
elseif( WIN32 )
	set( CMAKE_TOOLCHAIN_FILE $ENV{VCPKG_TOOLCHAIN_FILE} )
	set( CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /O2" )
endif()

project( vulkan-project )

include_directories( ./include )

if( APPLE )
	include_directories( /usr/local/include )
	link_directories( /usr/local/lib )
endif()

find_package( Threads REQUIRED )
find_package( Vulkan REQUIRED )

if( WIN32 )
	find_package( glm CONFIG REQUIRED )
	find_package( glfw3 CONFIG REQUIRED )
	find_package( Boost 1.73.0 REQUIRED )
endif()

file( GLOB SOURCES "./src/*.cpp" "./src/**/*.cpp" )

file( GLOB TEST_SOURCES "./src/**/*.cpp" "./test/*.cpp" "./test/**/*.cpp" )

add_executable( vulkan-game ${SOURCES} )

add_executable( vulkan-tests ${TEST_SOURCES} )

target_link_libraries( vulkan-game PRIVATE glfw Vulkan::Vulkan )

if( UNIX )
	target_link_libraries( vulkan-game PRIVATE dl )
endif()

target_link_libraries( vulkan-game PRIVATE Threads::Threads )

if( UNIX AND NOT APPLE )
	target_link_libraries( vulkan-game PRIVATE X11 Xxf86vm Xrandr Xi )
endif()

if( WIN32 )
	target_link_libraries( vulkan-tests PRIVATE Boost::boost )
endif()

