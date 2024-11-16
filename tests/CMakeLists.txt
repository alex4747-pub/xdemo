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

if(CMAKE_PROJECT_NAME STREQUAL PROJECT_NAME)
  include(cpplint)
endif()

find_package(GTest REQUIRED)

set(SRC_LIST main.cc test_helper.cc xdemo_test.cc)

add_executable(libtest ${SRC_LIST})
target_link_libraries(libtest libxdemo GTest::gtest GTest::gtest_main)
target_include_directories(libtest PRIVATE ${CMAKE_CURRENT_SOURCE_DIR})
add_test(NAME libtest COMMAND libtest)

# coverage test
add_executable(libtest_cov ${SRC_LIST})
target_link_libraries(libtest_cov libxdemo_cov GTest::gtest GTest::gtest_main)
target_include_directories(libtest_cov PRIVATE ${CMAKE_CURRENT_SOURCE_DIR})
# Note: there is no point to compile test files with coverage flag
target_link_options(libtest_cov PRIVATE -coverage)
add_test(NAME libtest_cov COMMAND libtest_cov)
