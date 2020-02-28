cmake_minimum_required(VERSION 3.16.2)

# TODO - SET THESE VARS
set(SOLUTION_NAME placeholder_soln)
set(PROJECT_NAME placeholder_proj)

# make empty folders
file(MAKE_DIRECTORY bin)
file(MAKE_DIRECTORY src)
file(MAKE_DIRECTORY lib)
file(MAKE_DIRECTORY include)

# 64-bit support
if ("${CMAKE_GENERATOR_PLATFORM}" STREQUAL "x64")
	set(BUILDING_64_BIT TRUE)
	message("Building 64-bit")
else()
	set(BUILDING_64_BIT FALSE)
	message("Building 32-bit")
endif()

# find all project sources
file(GLOB_RECURSE sources src/*.cpp src/*.h)

# set the project name
project(${SOLUTION_NAME})

# set libraries dir
link_directories( ${CMAKE_SOURCE_DIR}/lib )

# set CMake's build directories (archive dir = CMake artifacts dir?)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/bin)

# set build directories for each of VS's build types (Debug, Release, etc.)
foreach( OUTPUTCONFIG ${CMAKE_CONFIGURATION_TYPES} )
    string( TOUPPER ${OUTPUTCONFIG} OUTPUTCONFIG )
    set( CMAKE_ARCHIVE_OUTPUT_DIRECTORY_${OUTPUTCONFIG} ${CMAKE_SOURCE_DIR}/lib )
    set( CMAKE_LIBRARY_OUTPUT_DIRECTORY_${OUTPUTCONFIG} ${CMAKE_SOURCE_DIR}/lib )
    set( CMAKE_RUNTIME_OUTPUT_DIRECTORY_${OUTPUTCONFIG} ${CMAKE_SOURCE_DIR}/bin )
endforeach( OUTPUTCONFIG CMAKE_CONFIGURATION_TYPES )

# add source files (so they compile)
add_executable(${PROJECT_NAME} WIN32 ${sources})

# link executable to libraries
set_property(TARGET ${PROJECT_NAME} PROPERTY DEBUG_POSTFIX _d) # _dab on 'em

# specify include directories
include_directories(include)

# set VS startup project
set_property(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} PROPERTY VS_STARTUP_PROJECT ${PROJECT_NAME})

# set VS working directory to same place as binary files, so that relative file reading/writing has the same effects in debug mode
set_property(TARGET ${PROJECT_NAME} PROPERTY VS_DEBUGGER_WORKING_DIRECTORY "${CMAKE_SOURCE_DIR}/bin")

# set to C++ 17 (we doin this hardcore)
set_property(TARGET ${PROJECT_NAME} PROPERTY CXX_STANDARD 17)
