# Copyright (c) 2015 Advanced Micro Devices, Inc. All rights reserved.
# 
# Permission is hereby granted, free of charge, to any person obtaining a copy
# of this software and associated documentation files (the "Software"), to deal
# in the Software without restriction, including without limitation the rights
# to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
# copies of the Software, and to permit persons to whom the Software is
# furnished to do so, subject to the following conditions:
#  
# The above copyright notice and this permission notice shall be included in
# all copies or substantial portions of the Software.
#  
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.  IN NO EVENT SHALL THE
# AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
# OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
# THE SOFTWARE.

cmake_minimum_required (VERSION 2.8)
project (amdovx-modules)

list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)

find_package(OpenCV QUIET)
find_package(OpenCL QUIET)
find_package(FFmpeg QUIET)
find_package(miopengemm PATHS /opt/rocm)
find_package(miopen     PATHS /opt/rocm)

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_INSTALL_PREFIX /opt/rocm)

add_subdirectory(deps/amdovx-core)

if(OpenCL_FOUND)
    add_subdirectory (vx_loomsl)
    add_subdirectory (utils)
    if(FFMPEG_FOUND)
        add_subdirectory (vx_loomio_media)
    endif(FFMPEG_FOUND)
endif(OpenCL_FOUND)

if(OpenCL_FOUND AND miopengemm_FOUND AND miopen_FOUND)
    add_subdirectory (vx_nn)
    add_subdirectory (utils/annInferenceServer)
endif()

if(OpenCV_FOUND)
    if(${OpenCV_VERSION_MAJOR} EQUAL 3)
        add_subdirectory (vx_opencv)
    endif()
endif(OpenCV_FOUND)
