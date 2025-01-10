cmake_minimum_required(VERSION 3.10)
project(isotp LANGUAGES C)

include(AddGitSubmodule.cmake)

# Define the source files
set(ISOTP_SOURCES 
	${CMAKE_CURRENT_SOURCE_DIR}/src/isotp/allocate.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/isotp/isotp.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/isotp/receive.c
	${CMAKE_CURRENT_SOURCE_DIR}/src/isotp/send.c
)


# Define common include directories
set(COMMON_INCLUDE_ISOTP
    ${CMAKE_CURRENT_SOURCE_DIR}/src/isotp
)

add_git_submodule("deps/bitfield-c")

# Add the UDS library as a static library
add_library(isotp STATIC ${ISOTP_SOURCES})

target_link_libraries(isotp bitfield) ok so you need to setup a version of the two sub libraries and update them to use cmakelists for just themselves. then link them here

# Include directories for UDS
target_include_directories(isotp PUBLIC ${COMMON_INCLUDE_ISOTP})

# Compiler flags
target_compile_options(isotp PRIVATE
    -w                  # Disable all warnings
    -std=c99            # Use C99 standard
    -O0                 # No optimization
)

# Set the output directory for the library
set_target_properties(isotp PROPERTIES 
    ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/lib
)

