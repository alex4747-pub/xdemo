# Copyright (C) 2024  Aleksey Romanov (aleksey at voltanet dot io)
#
# Permission is hereby granted, free of charge, to any person obtaining
# a copy of this software and associated documentation files (the
# "Software"), to deal in the Software without restriction, including
# without limitation the rights to use, copy, modify, merge, publish,
# distribute, sublicense, and/or sell copies of the Software, and to
# permit persons to whom the Software is furnished to do so, subject to
# the following conditions:
#
# The above copyright notice and this permission notice shall be
# included in all copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
# EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
# MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
# NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
# LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
# OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
# WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

cmake_minimum_required(VERSION 3.20...3.30)

list(INSERT CMAKE_MODULE_PATH 0 "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

# Project name and a few useful settings. Other commands can pick up the results
project(
  XDemo
  VERSION 0.1
  DESCRIPTION "CMake learning"
  LANGUAGES CXX)

# Only do these if this is the main project, and not if it is included through add_subdirectory
if(CMAKE_PROJECT_NAME STREQUAL PROJECT_NAME)
  # Testing only available if this is the main app
  # Note this needs to be done in the main CMakeLists
  # since it calls enable_testing, which must be in the
  # main CMakeLists.
  include(CTest)

  # Docs only available if this is the main app
  find_package(Doxygen)
  if(Doxygen_FOUND)
    message(STATUS "Doxygen found, building docs")
    add_subdirectory(docs)
  else()
    message(STATUS "Doxygen not found, not building docs")
  endif()
endif()

# The compiled library code is here
add_subdirectory(src)

# The executable code is here
add_subdirectory(apps)

# Testing and code coveraee are only available
# if this is the main app
if((CMAKE_PROJECT_NAME STREQUAL PROJECT_NAME)
   AND BUILD_TESTING)
  add_subdirectory(tests)

  include(lcov)

  if(LCOV_FOUND)
    add_custom_target(lib_lcov
    COMMAND tests/libtest_cov
    COMMAND ${LCOV} --capture --directory src/CMakeFiles/libxdemo_cov.dir --output-file src/libxdemo_cov.info --test-name libtest_cov
    COMMAND rm -rf src/libxdemo_coverage_report
    COMMAND ${GENHTML} src/libxdemo_cov.info --output-directory src/libxdemo_coverage_report --title "Lib coverage report" --show-details --legend
    COMMAND cmake -E echo "Coverage report: ${CMAKE_BINARY_DIR}/src/libxdemo_coverage_report/index.html")

    add_custom_target(app_lcov
    COMMAND apps/xdemo_cov
    COMMAND ${LCOV} --capture --directory apps/CMakeFiles/xdemo_cov.dir --output-file apps/xdemo_cov.info --test-name xdemo_cov
    COMMAND ${LCOV} --remove apps/xdemo_cov.info "/usr*"  --output-file apps/xdemo_cov.info
    COMMAND cmake -E remove_directory apps/xdemo_coverage_report
    COMMAND ${GENHTML} apps/xdemo_cov.info --output-directory apps/xdemo_coverage_report --title "App coverage report" --show-details --legend
    COMMAND cmake -E echo "Coverage report: ${CMAKE_BINARY_DIR}/apps/xdemo_coverage_report/index.html")
  endif()
endif()
