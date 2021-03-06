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

add_subdirectory(rtklib)

set(GNSS_SPLIBS_SOURCES
    gps_l2c_signal.cc
    gps_l5_signal.cc
    galileo_e1_signal_processing.cc
    gnss_sdr_valve.cc
    gnss_sdr_sample_counter.cc
    gnss_signal_processing.cc
    gps_sdr_signal_processing.cc
    glonass_l1_signal_processing.cc
    glonass_l2_signal_processing.cc
    pass_through.cc
    galileo_e5_signal_processing.cc
    complex_byte_to_float_x2.cc
    byte_x2_to_complex_byte.cc
    cshort_to_float_x2.cc
    short_x2_to_cshort.cc
    complex_float_to_complex_byte.cc
    conjugate_cc.cc
    conjugate_sc.cc
    conjugate_ic.cc
    gnss_sdr_create_directory.cc
    geofunctions.cc
)

set(GNSS_SPLIBS_HEADERS
    gps_l2c_signal.h
    gps_l5_signal.h
    galileo_e1_signal_processing.h
    gnss_sdr_valve.h
    gnss_sdr_sample_counter.h
    gnss_signal_processing.h
    gps_sdr_signal_processing.h
    glonass_l1_signal_processing.h
    glonass_l2_signal_processing.h
    pass_through.h
    galileo_e5_signal_processing.h
    complex_byte_to_float_x2.h
    byte_x2_to_complex_byte.h
    cshort_to_float_x2.h
    short_x2_to_cshort.h
    complex_float_to_complex_byte.h
    conjugate_cc.h
    conjugate_sc.h
    conjugate_ic.h
    gnss_sdr_create_directory.h
    gnss_circular_deque.h
    geofunctions.h
)

include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_SOURCE_DIR}/src/core/system_parameters
    ${CMAKE_SOURCE_DIR}/src/core/receiver
    ${CMAKE_SOURCE_DIR}/src/core/interfaces
    ${Boost_INCLUDE_DIRS}
    ${GLOG_INCLUDE_DIRS}
    ${GFlags_INCLUDE_DIRS}
    ${ARMADILLO_INCLUDE_DIRS}
    ${GNURADIO_RUNTIME_INCLUDE_DIRS}
    ${GNURADIO_BLOCKS_INCLUDE_DIRS}
    ${VOLK_INCLUDE_DIRS}
    ${VOLK_GNSSSDR_INCLUDE_DIRS}
)


add_definitions(-DGNSSSDR_INSTALL_DIR="${CMAKE_INSTALL_PREFIX}")

list(SORT GNSS_SPLIBS_HEADERS)
list(SORT GNSS_SPLIBS_SOURCES)

add_library(gnss_sp_libs ${GNSS_SPLIBS_SOURCES} ${GNSS_SPLIBS_HEADERS})
source_group(Headers FILES ${GNSS_SPLIBS_HEADERS})

target_link_libraries(gnss_sp_libs
    ${GNURADIO_RUNTIME_LIBRARIES}
    ${VOLK_LIBRARIES} ${ORC_LIBRARIES}
    ${VOLK_GNSSSDR_LIBRARIES}  ${ORC_LIBRARIES}
    ${GFlags_LIBS}
    ${ARMADILLO_LIBRARIES}
    ${GNURADIO_BLOCKS_LIBRARIES}
    ${GNURADIO_FFT_LIBRARIES}
    ${GNURADIO_FILTER_LIBRARIES}
    ${OPT_LIBRARIES}
    gnss_rx
)

if(NOT VOLKGNSSSDR_FOUND)
    add_dependencies(gnss_sp_libs volk_gnsssdr_module
        armadillo-${armadillo_RELEASE})
else()
    add_dependencies(gnss_sp_libs armadillo-${armadillo_RELEASE})
endif()

if(${GFLAGS_GREATER_20})
    add_definitions(-DGFLAGS_GREATER_2_0=1)
endif()

add_library(gnss_sdr_flags gnss_sdr_flags.cc gnss_sdr_flags.h)
source_group(Headers FILES gnss_sdr_flags.h)
target_link_libraries(gnss_sdr_flags ${GFlags_LIBS})
