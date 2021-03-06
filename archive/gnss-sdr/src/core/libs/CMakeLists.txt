# Copyright (C) 2012-2018  (see AUTHORS file for a list of contributors)
#
# This file is part of GNSS-SDR.
#
# GNSS-SDR is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.
#
# GNSS-SDR is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with GNSS-SDR. If not, see <https://www.gnu.org/licenses/>.
#


if(OPENSSL_FOUND)
    add_definitions(-DUSE_OPENSSL_FALLBACK=1)
endif()

set(CORE_LIBS_SOURCES
    ini.cc
    INIReader.cc
    string_converter.cc
)

set(CORE_LIBS_HEADERS
    ini.h
    INIReader.h
    string_converter.h
)

include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_SOURCE_DIR}/src/core/system_parameters
    ${GLOG_INCLUDE_DIRS}
    ${GFlags_INCLUDE_DIRS}
    ${Boost_INCLUDE_DIRS}
    ${PUGIXML_INCLUDE_DIR}
)

list(SORT CORE_LIBS_HEADERS)
list(SORT CORE_LIBS_SOURCES)

add_library(rx_core_lib ${CORE_LIBS_SOURCES} ${CORE_LIBS_HEADERS})
source_group(Headers FILES ${CORE_LIBS_HEADERS})

if(PUGIXML_LOCAL)
    add_dependencies(rx_core_lib pugixml-${GNSSSDR_PUGIXML_LOCAL_VERSION})
endif()
