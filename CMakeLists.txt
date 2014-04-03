# Required for statically linking the MSVC runtime
project(cspice)
cmake_minimum_required(VERSION 2.8)

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)
file(GLOB_RECURSE HEADERS include/*.h)

file(GLOB_RECURSE CSUPPORT_SOURCE src/csupport/*.h src/csupport/*.c)
add_library(csupport STATIC  ${CSUPPORT_SOURCE} ${HEADERS})

file(GLOB_RECURSE CSPICE_SOURCE src/cspice/*.h src/cspice/*.c)
add_library(cspice STATIC ${CSPICE_SOURCE} ${HEADERS})

