################################################################################
# Project:  CMake4GDAL
# Purpose:  CMake build scripts
# Author:   Dmitry Baryshnikov, polimax@mail.ru
################################################################################
# Copyright (C) 2015-2016, NextGIS <info@nextgis.com>
# Copyright (C) 2012,2013,2014 Dmitry Baryshnikov
#
# Permission is hereby granted, free of charge, to any person obtaining a
# copy of this software and associated documentation files (the "Software"),
# to deal in the Software without restriction, including without limitation
# the rights to use, copy, modify, merge, publish, distribute, sublicense,
# and/or sell copies of the Software, and to permit persons to whom the
# Software is furnished to do so, subject to the following conditions:
#
# The above copyright notice and this permission notice shall be included
# in all copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS
# OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL
# THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
# FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER
# DEALINGS IN THE SOFTWARE.
################################################################################

option(ENABLE_GEORASTER "Build Oracle Spatial GeoRaster format" OFF)
if(ENABLE_GEORASTER AND OCI_FOUND)
    set(TARGET_NAME ras_oci)

    add_definitions(-DFRMT_georaster)
    set(FORMAT_DEFINITIONS ${FORMAT_DEFINITIONS} "-DFRMT_georaster" PARENT_SCOPE)

    include_directories(${CMAKE_CURRENT_SOURCE_DIR})
    include_directories(${OCI_INCLUDE_DIRS}/oci)

    set(LIB_CSOURCES
        cpl_vsil_ocilob.cpp
        georaster_dataset.cpp
        georaster_rasterband.cpp
        georaster_wrapper.cpp
        oci_wrapper.cpp
    )

    add_library(${TARGET_NAME} OBJECT ${LIB_CSOURCES})

    # build target
    set(TARGET_OBJECTS ${TARGET_OBJECTS} $<TARGET_OBJECTS:${TARGET_NAME}> PARENT_SCOPE)
    set(TARGET_LINK_LIB ${TARGET_LINK_LIB} PARENT_SCOPE)
    set(DEPENDENCY_LIB ${DEPENDENCY_LIB} PARENT_SCOPE)
else()
    warning_msg("Skip georaster")
endif()
