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


set(SRC_LIST xdemo_main.cc)

add_executable(xdemo ${SRC_LIST})
target_link_libraries(xdemo PRIVATE libxdemo)
target_compile_features(xdemo PRIVATE cxx_std_17)

# Define coverage variant
add_executable(xdemo_cov ${SRC_LIST})

# Note: we are linking non-cov library to separate
# coverage collection
target_link_libraries(xdemo_cov PRIVATE libxdemo)
target_compile_features(xdemo_cov PRIVATE cxx_std_17)
target_compile_options(xdemo_cov PRIVATE -coverage)
target_link_options(xdemo_cov PRIVATE -coverage)


  
