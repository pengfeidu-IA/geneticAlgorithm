# CMake script to build geneticAlgorithm

cmake_minimum_required(VERSION 3.0)

project(geneticAlgorithm)

# Enable verbose output.
#set(CMAKE_VERBOSE_MAKEFILE ON)

# Use C++11 standard.
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Find OpenMP.
find_package(OpenMP)
if (OPENMP_FOUND)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS} -fPIC -Wall -Wextra -Werror -pedantic")
else()
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC -Wall -Wextra -Werror -pedantic")
endif()

if (COV)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} --coverage")
endif()

# MPI.
if (MPI)
    message(STATUS "-- This is a parallel MPI build --")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${MPI_CXX_COMPILER_FLAGS}")
    include_directories(${MPI_CXX_INCLUDE_DIRS})
    file(WRITE ${PROJECT_SOURCE_DIR}/src/mpiflag.h "#define RUNMPI true\n")
else()
    message(STATUS "-- This is a serial build --")
    file(WRITE ${PROJECT_SOURCE_DIR}/src/mpiflag.h "#define RUNMPI false\n")
endif()

# find libconfig library
find_package(Config++)
if (NOT ${CONFIG++_FOUND})
  message(FATAL_ERROR "Libconfig++ could not be located.")
else()
    include_directories(${CONFIG++_INCLUDE_DIR})
endif()

include_directories(${CONFIG++_INCLUDE_DIR})

# geneticAlgorithm source dir.
add_subdirectory(src)


# geneticAlgorithm run dir.
add_subdirectory(example)

