################################################################################
# This file is part of the argtable3 library.
#
# Copyright (C) 2016-2017 Tom G. Huang
# <tomghuang@gmail.com>
# All rights reserved.
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions are met:
#     * Redistributions of source code must retain the above copyright
#       notice, this list of conditions and the following disclaimer.
#     * Redistributions in binary form must reproduce the above copyright
#       notice, this list of conditions and the following disclaimer in the
#       documentation and/or other materials provided with the distribution.
#     * Neither the name of STEWART HEITMANN nor the  names of its contributors
#       may be used to endorse or promote products derived from this software
#       without specific prior written permission.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
# AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
# IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
# ARE DISCLAIMED. IN NO EVENT SHALL STEWART HEITMANN BE LIABLE FOR ANY DIRECT,
# INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES
# (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
# LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND
# ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
# (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
# SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
################################################################################

cmake_minimum_required(VERSION 2.8.12)
project(argtable3)

option(argtable3_build_tests "Build argtable3 unit tests." OFF)
option(argtable3_build_examples "Build argtable3 examples." OFF)
option(argtable3_build_static_lib "Build static libargtable3" OFF)

if(argtable3_build_tests)
  add_subdirectory(tests)
endif()

if(argtable3_build_examples)
  add_subdirectory(examples)
endif()

set (warning_flags "-Wall -pedantic")
set (c_language_flags "-std=gnu11 -g")
set (cxx_language_flags "-std=c++11 -g")
set (CMAKE_C_FLAGS  "${CMAKE_C_FLAGS} ${warning_flags} ${c_language_flags}" )
set (CMAKE_CXX_FLAGS  "${CMAKE_CXX_FLAGS} ${warning_flags} ${cxx_language_flags}" )

set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR}/lib)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR}/lib)

set(ARGTABLE_SRC
    argtable3.c)

set(ARGTABLE_HDR
    argtable3.h)

source_group("Headers" FILES ${ARGTABLE_HDR})
source_group("Source" FILES ${ARGTABLE_SRC})

if (argtable3_build_static_lib)
    add_library(argtable STATIC ${ARGTABLE_SRC})
    target_link_libraries(argtable :libdl.so)
    install(TARGETS argtable DESTINATION lib)
endif()

# Install headers.
install(FILES ${ARGTABLE_HDR} DESTINATION include)

# And copy headers to build dir.
configure_file(argtable3.h ${PROJECT_BINARY_DIR}/include/argtable3.h COPYONLY)
include_directories("${PROJECT_BINARY_DIR}/include/")

