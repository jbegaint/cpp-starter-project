cmake_minimum_required(VERSION 3.2)

project(cpp-project CXX)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/bin)

option(BUILD_SHARED_LIBS OFF "Build shared libraries")

if(NOT CMAKE_BUILD_TYPE)
	set(CMAKE_BUILD_TYPE Debug)
endif()

if (NOT BUILD_SHARED_LIBS)
	if(WIN32)
		set(CMAKE_FIND_LIBRARY_SUFFIXES .lib .a ${CMAKE_FIND_LIBRARY_SUFFIXES})
	else()
		set(CMAKE_FIND_LIBRARY_SUFFIXES .a ${CMAKE_FIND_LIBRARY_SUFFIXES})
	endif()
endif()


# Extra cmake modules
list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/cmake")

# find_package(SomeLib REQUIRED)


add_executable(hello-world ${CMAKE_CURRENT_SOURCE_DIR}/src/hello.cpp)
target_include_directories(hello-world PRIVATE 
	${CMAKE_CURRENT_SOURCE_DIR}/include)
# target_link_libraries(hello-world ...)

if (NOT BUILD_SHARED_LIBS)
	target_link_libraries(hello-world -static-libgcc -static-libstdc++)
endif()

target_compile_options(hello-world PRIVATE -std=c++14 -Wall -Wextra)
if (CMAKE_BUILD_TYPE MATCHES Release)
	target_compile_options(hello-world PRIVATE -O3 -march=native)
endif()
