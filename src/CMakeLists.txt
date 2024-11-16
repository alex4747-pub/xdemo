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


# Note that headers are optional, and do not affect add_library, but they will not
# show up in IDEs unless they are listed in add_library.

set(HEADER_LIST
"${XDemo_SOURCE_DIR}/include/xdemo/xdemo.h")

set(SRC_LIST xdemo_a.cc xdemo_b.cc)

# Make an automatic library - will be static or dynamic based on user setting
add_library(libxdemo ${SRC_LIST} ${HEADER_LIST})

# We need this directory, and users of our library will need it too
target_include_directories(libxdemo PUBLIC ../include)

# All users of this library will need at least C++17
target_compile_features(libxdemo PUBLIC cxx_std_17)

# IDEs should put the headers in a nice place
source_group(
  TREE "${XDemo_SOURCE_DIR}/include"
  PREFIX "Header Files"
  FILES ${HEADER_LIST})

# Define coverage variant
add_library(libxdemo_cov ${SRC_LIST} ${HEADER_LIST})
target_include_directories(libxdemo_cov PUBLIC ../include)
target_compile_features(libxdemo_cov PUBLIC cxx_std_17)
target_compile_options(libxdemo_cov PRIVATE -coverage)
target_link_options(libxdemo_cov PRIVATE -coverage)

  
