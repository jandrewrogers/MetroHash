
# CMakeLists.txt
#
# Copyright 2015-2019 J. Andrew Rogers
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

cmake_minimum_required(VERSION 3.1)

project(MetroHash VERSION 1.0.0 LANGUAGES CXX)


add_library(metrohash STATIC "")

target_include_directories(metrohash PUBLIC inc)

target_sources(metrohash PRIVATE inc/metrohash/MetroHash.h
                                 inc/metrohash/MetroHash64.h
                                 inc/metrohash/MetroHash128.h
                                 inc/metrohash/MetroHash128crc.h
                                 src/MetroHash64.cpp
                                 src/MetroHash128.cpp
                                 src/MetroHash128crc.cpp
                                 src/Platform.h
                                 src/TestVector.h)


set_target_properties(metrohash PROPERTIES CXX_STANDARD          98
                                           CXX_STANDARD_REQUIRED ON
                                           CXX_EXTENSIONS        OFF)


if(CMAKE_CXX_COMPILER_ID MATCHES "GNU|Clang")

    # [GCC] x86 Options
    #   https://gcc.gnu.org/onlinedocs/gcc-8.1.0/gcc/x86-Options.html
    target_compile_options(metrohash PRIVATE
        -msse4.2) # Enable the use of SSE4.2 instructions.

    # [GCC] Options to Request or Suppress Warnings
    #   https://gcc.gnu.org/onlinedocs/gcc-8.1.0/gcc/Warning-Options.html
    target_compile_options(metrohash PRIVATE
        -Wall
        -Wextra
        -Wpedantic
        -Werror)

elseif(CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")

    # [MSVC] Minimum CPU Architecture
    #   https://docs.microsoft.com/en-us/cpp/build/reference/arch-x86
    #   https://docs.microsoft.com/en-us/cpp/build/reference/arch-x64
    # For x64 CPUs SSE4.2 instructions are enabled by default.

    # [MSVC] Warning Level
    #    https://docs.microsoft.com/en-us/cpp/build/reference/compiler-option-warning-level
    target_compile_options(metrohash PRIVATE
        /W4  # Enable warning level 4.
        /WX) # Treat warnings as errors.

else()
    message(FATAL_ERROR "Compiler ${CMAKE_CXX_COMPILER_ID} is not supported!")
endif()


if(NOT CMAKE_SIZEOF_VOID_P EQUAL 8)

    message(FATAL_ERROR "MetroHash supports only 64-bit builds!")

endif()
