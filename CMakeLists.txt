# Required for statically linking the MSVC runtime
set(CMAKE_USER_MAKE_RULES_OVERRIDE
   ${CMAKE_CURRENT_SOURCE_DIR}/cmake/c_flag_overrides.cmake)
set(CMAKE_USER_MAKE_RULES_OVERRIDE_CXX
   ${CMAKE_CURRENT_SOURCE_DIR}/cmake/cxx_flag_overrides.cmake)

project(cspice)
cmake_minimum_required(VERSION 2.8)
include(GenerateExportHeader)

set(CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)
set(CMAKE_VERBOSE_MAKEFILE ON)
set(CMAKE_BUILD_TYPE DEBUG)
set_property(GLOBAL PROPERTY USE_FOLDERS ON)
set_property(GLOBAL PROPERTY PREDEFINED_TARGETS_FOLDER "CMakeTargets")
set(TARGET_ARCHITECTURE "x64")

if(CMAKE_COMPILER_IS_GNUCXX)
    # Ensure we use C++ 11 
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
    # Ensure we generate position independent code 
    if("${CMAKE_SYSTEM_PROCESSOR}" STREQUAL "x86_64")
        set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -fPIC"  )
        set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -fPIC"  )    
    endif()
endif()

if(APPLE)
    # Ensure we use C++ 11 
    set(CMAKE_XCODE_ATTRIBUTE_CLANG_CXX_LANGUAGE_STANDARD "c++0x")
    set(CMAKE_XCODE_ATTRIBUTE_CLANG_CXX_LIBRARY "libc++")
    set(CMAKE_XCODE_ATTRIBUTE_GCC_VERSION "com.apple.compilers.llvm.clang.1_0")
endif()

if(WIN32)
    add_definitions(-DUNICODE -D_UNICODE)
    set(TARGET_ARCHITECTURE "x86")
endif()

include_directories(${CMAKE_SOURCE_DIR}/include)
file(GLOB_RECURSE HEADERS include/*.h)

file(GLOB_RECURSE CSUPPORT_SOURCE src/csupport/*.h src/csupport/*.c)
add_library(csupport STATIC ${CSUPPORT_SOURCE} ${HEADERS})
#message(FATAL_ERROR ${CSUPPORT_SOURCE})
file(GLOB_RECURSE CSPICE_SOURCE src/cspice/*.h src/cspice/*.c)
add_library(cspice STATIC ${CSPICE_SOURCE} ${HEADERS})

