#
# SPDX-FileCopyrightText: Copyright 2010-2023 Arm Limited and/or its affiliates <open-source-office@arm.com>
#
# SPDX-License-Identifier: Apache-2.0
#
# Licensed under the Apache License, Version 2.0 (the License); you may
# not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an AS IS BASIS, WITHOUT
# WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

cmake_minimum_required(VERSION 3.15.6)

project(CMSISNN)

# Add an option for choosing between building from source or using a precompiled library
# option(ENABLE_CMSIS_NN_BUILD "Build cmsis-nn from source" ON) # OR cmake -B build -DENABLE_CMSIS_NN_BUILD=1

if(ENABLE_CMSIS_NN_BUILD)
    # Build from source
    add_library(cmsis-nn STATIC)
    target_compile_options(cmsis-nn PRIVATE -Ofast)
    target_include_directories(cmsis-nn PUBLIC "Include")
    add_subdirectory(Source)
else()
    # Use precompiled library
    add_library(cmsis-nn STATIC IMPORTED GLOBAL)
    set_target_properties(cmsis-nn PROPERTIES
        IMPORTED_LOCATION ${CMAKE_CURRENT_SOURCE_DIR}/lib/libcmsis-nn.a)
    # target_compile_options(cmsis-nn INTERFACE -Ofast)
    target_include_directories(cmsis-nn INTERFACE 
        ${CMAKE_CURRENT_SOURCE_DIR}/Include
    )
    
endif()
