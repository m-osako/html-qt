# CMakeLists for libhtml-qt
project(html-qt)

cmake_minimum_required(VERSION 2.8.6 FATAL_ERROR)
find_package(PkgConfig REQUIRED)
find_package(Qt5 5.3.0 COMPONENTS
    Core
    Network
)

set(HTMLQT_VERSION_MAJOR  "0")
set(HTMLQT_VERSION_MINOR  "1")
set(HTMLQT_VERSION_PATCH  "0")
set(HTMLQT_VERSION_SUFFIX "${VERSION_SUFFIX}")
set(HTMLQT_VERSION "${HTMLQT_VERSION_MAJOR}.${HTMLQT_VERSION_MINOR}.${HTMLQT_VERSION_PATCH}")

set(HTMLQT_API_LEVEL "0")

# CMakeLists for HTML-Qt library
set(CMAKE_AUTOMOC ON)

# Include our cmake modules
set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake/modules)

# As moc files are generated in the binary dir, tell CMake
# to always look for includes there:
set(CMAKE_INCLUDE_CURRENT_DIR ON)

# Forbid in-tree building
if(${CMAKE_SOURCE_DIR} MATCHES ${CMAKE_BINARY_DIR})
      message(STATUS "Please do an out-of-tree build:")
      message(STATUS "rm -f CMakeCache.txt && mkdir build && cd build; cmake .. && make")
      message(FATAL_ERROR "In-tree-build detected!")
endif(${CMAKE_SOURCE_DIR} MATCHES ${CMAKE_BINARY_DIR})

#
# Options
#

# NONE

if(CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
  set(CMAKE_INSTALL_PREFIX
    "/usr" CACHE PATH "html-qt default install prefix" FORCE)
endif(CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)

#
# Configure files
#
set (PREFIXDIR "${CMAKE_INSTALL_PREFIX}")
set (CMAKECONFIG_INSTALL_DIR "${CMAKE_INSTALL_LIBDIR}/cmake/${LIBNAME}/")
set (DATADIR "${CMAKE_INSTALL_PREFIX}/share")
set (PKGDATADIR "${DATA_INSTALL_DIR}")
set (LIBDIR "${CMAKE_INSTALL_PREFIX}/${LIB_INSTALL_DIR}")
set (PKGLIBDIR "${LIBDIR}/html-qt")
set (GETTEXT_PACKAGE "html-qt")
set (LOCALE_DIR "${DATADIR}/locale")
set (VERSION "${HTMLQT_VERSION}")
set (BUILDDIR "${CMAKE_BINARY_DIR}")

add_definitions("-DLOCALSTATEDIR=\"${LOCALSTATEDIR}\"")

set(CMAKE_INSTALL_LIBDIR "${CMAKE_INSTALL_PREFIX}/lib/${CMAKE_LIBRARY_ARCHITECTURE}" CACHE PATH "Output directory for libraries")

configure_file(config.h.in ${CMAKE_BINARY_DIR}/config.h)

#
# Custom C flags
#
set (MAINTAINER_CFLAGS "-Werror -Wall -Wcast-align -Wno-uninitialized -Wempty-body -Wformat-security -Wformat -Winit-self")
option (DISABLE_MAINTAINER_CFLAGS "Disable maintainer CFlags" ON)
if (DISABLE_MAINTAINER_CFLAGS)
	set (MAINTAINER_CFLAGS "")
endif (DISABLE_MAINTAINER_CFLAGS)
add_definitions(${MAINTAINER_CFLAGS})

add_definitions(-DQT_NO_KEYWORDS)

include_directories(
    ${CMAKE_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/lib
    ${CMAKE_CURRENT_BINARY_DIR}
)

add_subdirectory(html-qt)
add_subdirectory(cmd)
