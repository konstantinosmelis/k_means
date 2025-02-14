cmake_minimum_required(VERSION 3.16..)
project(k_means)

# CXX flags
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_FLAGS    "-Wall -Wextra -O2")

# Useful variables
set(HEADERS_DIR         "Includes")
set(SOURCES_DIR         "Sources")
set(DEPENDENCIES_DIR    "Dependencies")

# Dependencies
set(NANOFLANN_DEPENDENCY_DIR "${DEPENDENCIES_DIR}/nanoflann/")

# Project Sources
file(GLOB_RECURSE HEADERS "${HEADERS_DIR}/*")
file(GLOB_RECURSE SOURCES "${SOURCES_DIR}/*")
add_library(k_means SHARED ${HEADERS} ${SOURCES})

# Compilation options
set(CMAKE_CONFIGURATION_TYPES "Debug;Release;RelWithDebInfo")
target_compile_options(k_means PRIVATE
        $<$<CONFIG:Debug>:${CMAKE_CXX_FLAGS} -g -O0>
        $<$<CONFIG:Release>:${CMAKE_CXX_FLAGS} -O3 -DNDEBUG>
        $<$<CONFIG:RelWithDebInfo>:${CMAKE_CXX_FLAGS} -g>
)

# Project includes
target_include_directories(k_means PRIVATE "${HEADERS_DIR}")
target_include_directories(k_means PRIVATE "${NANOFLANN_DEPENDENCY_DIR}/include/")

# Precomp
if(MSVC)
    add_msvc_precompile_headers("${HEADERS_DIR}/Precomp.h" "${SOURCES_DIR}/Precomp.h")
else()
    target_precompile_headers(k_means PRIVATE "${HEADERS_DIR}/Precomp.h" "${SOURCES_DIR}/Precomp.cpp")
endif()
