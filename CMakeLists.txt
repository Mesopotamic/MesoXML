cmake_minimum_required(VERSION 3.16.0 FATAL_ERROR)
project(MesoXML VERSION 0.1.0 LANGUAGES C)

# CMake used for every Mesopotamic project ========================

# Builds can be ran for just Solomon or the Mesopotamic root
# The dependencies have different locations based on where the
# build originates from, cache the component variables
# If build root is mesopotamic then these caches won't overwrite
set(MESO_XML ${CMAKE_CURRENT_LIST_DIR} CACHE PATH "")
set(MESO_CMAKE ${CMAKE_CURRENT_LIST_DIR}/CMake CACHE PATH "")

# Has the user specificed a static build of the library?
# This is done when working on Mesopotamic components 
# because you don't have to copy the components across
# Although by default we build shared libraries with 
# all their symbols exported
if(${MESO_BUILD_STATIC})
    set(BUILD_TYPE "")
else()
    set(BUILD_TYPE "SHARED")
    set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS ON)
endif()

# # Stop the MesoXML project from getting added multiple times
if(TARGET MesoXML)
    return()
endif()

# Add in the CMake helpers used by all mesopotamic projects
list(APPEND CMAKE_MODULE_PATH ${MESO_CMAKE})
include(Mesopotamic)
# =================================================================

# Add the library
add_library(MesoXML ${BUILD_TYPE} Source/MesoXML.c Include/MesoXML.h)

# Set the include directories for MesoXML
set(MESOXML_include_dirs ${CMAKE_CURRENT_LIST_DIR}/Include CACHE PATH "Path to MesoXML header")
target_include_directories(MesoXML PUBLIC ${MESOXML_include_dirs})

# Add the examples if specified
if(${MESO_BUILD_EXAMPLES})
	message(STATUS "Adding MesoXML examples")
	add_subdirectory(Examples)
endif()

# Sort the Files
meso_sort_target(MesoXML)