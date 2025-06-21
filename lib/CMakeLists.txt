# RGB Matrix Library CMakeLists.txt

# Source files
set(RGBMATRIX_SOURCES
    gpio.cc
    led-matrix.cc
    options-initialize.cc
    framebuffer.cc
    thread.cc
    bdf-font.cc
    graphics.cc
    led-matrix-c.cc
    hardware-mapping.c
    multiplex-mappers.cc
    pixel-mapper.cc
    content-streamer.cc
)

# Headers
set(RGBMATRIX_HEADERS
    framebuffer-internal.h
    gpio-bits.h
    gpio.h
    hardware-mapping.h
    multiplex-mappers-internal.h
    utf8-internal.h
)

# Create static library
add_library(rgbmatrix_static STATIC ${RGBMATRIX_SOURCES})

# Create shared library
add_library(rgbmatrix_shared SHARED ${RGBMATRIX_SOURCES})

# Set output names
set_target_properties(rgbmatrix_static PROPERTIES OUTPUT_NAME "rgbmatrix")
set_target_properties(rgbmatrix_shared PROPERTIES 
    OUTPUT_NAME "rgbmatrix"
    VERSION "1.0.0"
    SOVERSION "1"
)

# Include directories
target_include_directories(rgbmatrix_static PUBLIC ../include)
target_include_directories(rgbmatrix_shared PUBLIC ../include)

# Compiler flags
target_compile_options(rgbmatrix_static PRIVATE 
    -W -Wall -Wextra -Wno-unused-parameter -O3 -g -fPIC -march=native
)
target_compile_options(rgbmatrix_shared PRIVATE 
    -W -Wall -Wextra -Wno-unused-parameter -O3 -g -fPIC -march=native
)

# C++ standard
set_target_properties(rgbmatrix_static PROPERTIES CXX_STANDARD 11)
set_target_properties(rgbmatrix_shared PROPERTIES CXX_STANDARD 11)

# Default hardware description
if(NOT DEFINED HARDWARE_DESC)
    set(HARDWARE_DESC "regular")
endif()

# Add compile definitions
target_compile_definitions(rgbmatrix_static PRIVATE 
    DEFAULT_HARDWARE="${HARDWARE_DESC}"
)
target_compile_definitions(rgbmatrix_shared PRIVATE 
    DEFAULT_HARDWARE="${HARDWARE_DESC}"
)

# Link libraries
target_link_libraries(rgbmatrix_shared pthread rt m)

# Create an alias for the library
add_library(rgbmatrix ALIAS rgbmatrix_static)

# Install targets
install(TARGETS rgbmatrix_static rgbmatrix_shared
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    RUNTIME DESTINATION bin
)

# Install headers
install(DIRECTORY ../include/ DESTINATION include
    FILES_MATCHING PATTERN "*.h"
)