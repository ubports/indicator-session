project (indicator-session C CXX)
cmake_minimum_required (VERSION 2.8.9)

set (PROJECT_VERSION "13.10.1")
set (PACKAGE ${CMAKE_PROJECT_NAME})
set (GETTEXT_PACKAGE indicator-session)

##
##  GNU standard installation directories
## 
include (GNUInstallDirs)
if (EXISTS "/etc/debian_version") # Workaround for libexecdir on debian
  set (CMAKE_INSTALL_LIBEXECDIR "${CMAKE_INSTALL_LIBDIR}")
  set (CMAKE_INSTALL_FULL_LIBEXECDIR "${CMAKE_INSTALL_FULL_LIBDIR}")
endif ()
set (CMAKE_INSTALL_PKGLIBEXECDIR "${CMAKE_INSTALL_LIBEXECDIR}/${CMAKE_PROJECT_NAME}")
set (CMAKE_INSTALL_FULL_PKGLIBEXECDIR "${CMAKE_INSTALL_FULL_LIBEXECDIR}/${CMAKE_PROJECT_NAME}")


##
##  Check for prerequisites
##
find_package (PkgConfig REQUIRED)
include (FindPkgConfig)
pkg_check_modules (SERVICE REQUIRED
                   glib-2.0>=2.36
                   gio-unix-2.0>=2.36
                   url-dispatcher-1>=1
                   libwhoopsie)
include_directories(SYSTEM ${SERVICE_INCLUDE_DIRS})

set (CC_WARNING_ARGS " -Wall -pedantic -Wextra -Wno-missing-field-initializers")

set (ARCHIVE_NAME ${CMAKE_PROJECT_NAME}-${PROJECT_VERSION})
add_custom_target (dist
                   COMMAND bzr export --root=${ARCHIVE_NAME} ${CMAKE_BINARY_DIR}/${ARCHIVE_NAME}.tar.gz
                   WORKING_DIRECTORY ${CMAKE_SOURCE_DIR})

add_custom_target (cppcheck COMMAND cppcheck --enable=all -q --error-exitcode=2 --inline-suppr
                   ${CMAKE_SOURCE_DIR}/src
                   ${CMAKE_SOURCE_DIR}/tests)

include_directories (${CMAKE_CURRENT_SOURCE_DIR}/src)
include_directories (${CMAKE_CURRENT_BINARY_DIR}/src)

add_subdirectory (src)
add_subdirectory (data)
add_subdirectory (po)

# testing & coverage
enable_testing ()
add_subdirectory (tests)
find_package(CoverageReport)
ENABLE_COVERAGE_REPORT(
  TARGETS ${COVERAGE_TARGETS}
  TESTS ${COVERAGE_TEST_TARGETS}
  FILTER /usr/include ${CMAKE_BINARY_DIR}/*
)
