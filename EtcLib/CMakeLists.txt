# Copyright 2015 The Etc2Comp Authors.
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

project(EtcLib)
include_directories(./Etc)
include_directories(./EtcCodec)

file(GLOB HDR_SOURCES
	${PROJECT_SOURCE_DIR}/Etc/*.h
	${PROJECT_SOURCE_DIR}/EtcCodec/*.h)

file(GLOB CPP_SOURCES
	${PROJECT_SOURCE_DIR}/Etc/*.cpp
	${PROJECT_SOURCE_DIR}/EtcCodec/*.cpp)

ADD_LIBRARY(EtcLib ${CPP_SOURCES} ${HDR_SOURCES})
TARGET_INCLUDE_DIRECTORIES(EtcLib PUBLIC
        $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/Etc>
        $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/EtcCodec>
        $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}/etc2comp/${PROJECT_NAME}/Etc>
        $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}/etc2comp/${PROJECT_NAME}/EtcCodec>)
TARGET_LINK_LIBRARIES(EtcLib PUBLIC Threads::Threads)

INSTALL(TARGETS EtcLib
    EXPORT "${TARGETS_EXPORT_NAME}"
    INCLUDES DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}"
    LIBRARY DESTINATION "${CMAKE_INSTALL_LIBDIR}"
    ARCHIVE DESTINATION "${CMAKE_INSTALL_LIBDIR}"
    RUNTIME DESTINATION "${CMAKE_INSTALL_BINDIR}")

INSTALL(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
        DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/etc2comp
        FILES_MATCHING PATTERN "*.h")
