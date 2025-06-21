cmake_minimum_required(VERSION 3.12)
project(rgbmatrix 
    VERSION 1.0.0
    DESCRIPTION "RGB LED matrix library"
    LANGUAGES C CXX)

# Options
option(BUILD_PYTHON_BINDINGS "Build Python bindings" OFF)
option(BUILD_CSHARP_BINDINGS "Build C# bindings" OFF)
option(BUILD_EXAMPLES "Build example applications" ON)
option(BUILD_UTILS "Build utility applications" ON)

# Add the library subdirectory first
add_subdirectory(lib)

# Conditionally add other components
if(BUILD_EXAMPLES)
    add_subdirectory(examples-api-use)
endif()

if(BUILD_UTILS)
    add_subdirectory(utils)
endif()

if(BUILD_PYTHON_BINDINGS)
    add_subdirectory(bindings/python)
endif()

if(BUILD_CSHARP_BINDINGS)
    add_subdirectory(bindings/c#)
endif()

