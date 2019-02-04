project(qtubuntu-media-signals)
cmake_minimum_required(VERSION 2.8.9)

set(SONAME 2)
set(VERSION 2.0)
set(SOVERSION 2.0.0)

# Instruct CMake to run moc automatically when needed.
set(CMAKE_AUTOMOC ON)

# Dependencies
include(FindPkgConfig)
# Standard install paths
include(GNUInstallDirs)

find_package(Qt5Core REQUIRED)
find_package(Qt5Gui REQUIRED)

add_definitions(-DWITHQT5=1)

set(QTUBUNTU_MEDIA_SIGNALS_LIBNAME qtubuntu-media-signals)
set(OUR_QT_INCLUDES ${Qt5Core_INCLUDE_DIRS} ${Qt5Gui_INCLUDE_DIRS})
set(OUR_QT_CORE_LIB ${Qt5Core_LIBRARIES} ${Qt5Gui_LIBRARIES})
set(QT_PKGCONFIG_DEPENDENCIES "Qt5Core Qt5Gui")

set(QTUBUNTU_MEDIA_SIGNALS_PKGCONFIG_FILE lib${QTUBUNTU_MEDIA_SIGNALS_LIBNAME}.pc)

# Build flags
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wundef")

# Sources
set(QTUBUNTU_MEDIA_SIGNALS_SRCS
    src/qtubuntu_media_signals.cpp
    )

# Build
add_definitions(-DQT_NO_KEYWORDS)

include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_BINARY_DIR}
    ${OUR_QT_INCLUDES}
    )

add_library(${QTUBUNTU_MEDIA_SIGNALS_LIBNAME} SHARED ${QTUBUNTU_MEDIA_SIGNALS_SRCS})
target_link_libraries(${QTUBUNTU_MEDIA_SIGNALS_LIBNAME}
    ${OUR_QT_CORE_LIB}
    )

set_target_properties(${QTUBUNTU_MEDIA_SIGNALS_LIBNAME} PROPERTIES
  SOVERSION ${SONAME}
  VERSION ${SOVERSION}
  )

# Install
set(INCLUDE_INSTALL_DIR include/lib${QTUBUNTU_MEDIA_SIGNALS_LIBNAME})

install(TARGETS ${QTUBUNTU_MEDIA_SIGNALS_LIBNAME}
    LIBRARY DESTINATION lib/${CMAKE_LIBRARY_ARCHITECTURE}${LIB_SUFFIX}
    )

install(FILES src/qtubuntu_media_signals.h
    DESTINATION ${INCLUDE_INSTALL_DIR}
    )

# PkgConfig file
set (PREFIX "${CMAKE_INSTALL_PREFIX}")
set (EXEC_PREFIX "${CMAKE_INSTALL_PREFIX}")

configure_file (libqtubuntu_media_signals.pc.in
    ${CMAKE_CURRENT_BINARY_DIR}/${QTUBUNTU_MEDIA_SIGNALS_PKGCONFIG_FILE} @ONLY)

install(FILES  ${CMAKE_CURRENT_BINARY_DIR}/${QTUBUNTU_MEDIA_SIGNALS_PKGCONFIG_FILE}
    DESTINATION ${CMAKE_INSTALL_PREFIX}/lib/${CMAKE_LIBRARY_ARCHITECTURE}/pkgconfig
    )
