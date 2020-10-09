# Copyright 2015 Etc2Comp Authors.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#   http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

cmake_minimum_required(VERSION 3.0)

include(cmake/HunterGate.cmake)
HunterGate(
    URL "https://github.com/cpp-pm/hunter/archive/v0.23.275.tar.gz"
    SHA1 "0952a54dc983ecbea09dfe8c53ee65d1e0e91232"
)

project(etc2comp VERSION 0.0.0)

option(BUILD_TOOL "Build EtcTool" ON)

#set (CMAKE_CXX_STANDARD 11)
#IF (APPLE)
#	set (CMAKE_CXX_FLAGS "-I/usr/include/i386-linux-gnu/c++/4.8 -I/usr/include/c++/4.8 -std=c++11 -g3 -Wall -O3")
#ELSE ()
#	IF (WIN32)
#		set (CMAKE_CXX_FLAGS "-I/usr/include/i386-linux-gnu/c++/4.8 -I/usr/include/c++/4.8 -W4 /EHsc")
#	ELSE()
#		set (CMAKE_CXX_FLAGS "-I/usr/include/i386-linux-gnu/c++/4.8 -I/usr/include/c++/4.8 -std=c++11 -pthread -g3 -Wall -O2")
#	ENDIF()
#ENDIF ()

# Introduce variables:
# * CMAKE_INSTALL_LIBDIR
# * CMAKE_INSTALL_BINDIR
# * CMAKE_INSTALL_INCLUDEDIR
include(GNUInstallDirs)

FIND_PACKAGE(Threads REQUIRED)

ADD_SUBDIRECTORY(EtcLib)
SET(TARGETS EtcLib)
IF(BUILD_TOOL)
  ADD_SUBDIRECTORY(EtcTool)
  LIST(APPEND TARGETS EtcTool)
ENDIF()

set(GENERATED_DIR "${CMAKE_CURRENT_BINARY_DIR}/generated")
set(VERSION_CONFIG "${GENERATED_DIR}/${PROJECT_NAME}ConfigVersion.cmake")
set(PROJECT_CONFIG "${GENERATED_DIR}/${PROJECT_NAME}Config.cmake")

set(CONFIG_INSTALL_DIR "${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}")

set(TARGETS_EXPORT_NAME "${PROJECT_NAME}Targets")
set(NAMESPACE "${PROJECT_NAME}::")

include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    "${VERSION_CONFIG}" COMPATIBILITY ExactVersion
)

configure_package_config_file(
    "cmake/Config.cmake.in"
    "${PROJECT_CONFIG}"
    INSTALL_DESTINATION "${CONFIG_INSTALL_DIR}"
)

install(
    TARGETS ${TARGETS}
    EXPORT "${TARGETS_EXPORT_NAME}"
    INCLUDES DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}"
    RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}"
    LIBRARY DESTINATION "${CMAKE_INSTALL_LIBDIR}"
    ARCHIVE DESTINATION "${CMAKE_INSTALL_LIBDIR}"
)

install(
    FILES "${PROJECT_CONFIG}" "${VERSION_CONFIG}"
    DESTINATION "${CONFIG_INSTALL_DIR}"
)

install(
    EXPORT "${TARGETS_EXPORT_NAME}"
    NAMESPACE "${NAMESPACE}"
    DESTINATION "${CONFIG_INSTALL_DIR}"
)
