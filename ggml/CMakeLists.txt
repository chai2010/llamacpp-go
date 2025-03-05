# Copyright 2025 <chaishushan{AT}gmail.com>. All rights reserved.
# Use of this source code is governed by a BSD-style
# license that can be found in the LICENSE file.

cmake_minimum_required (VERSION 3.0.0)

project(GGML)

#------------------------------------------------------------------------------

# Extra flags to enable experimental features and code
add_definitions(
  #-DWEBP_HAVE_AVX2
)

include_directories(AFTER
  ./include
  ./src
)

set(GGML_SRC
  #./src/ggml-alloc.c
  #./src/ggml-quants.c
  #./src/ggml.c
#
  #./src/ggml-backend-reg.cpp
  #./src/ggml-backend.cpp
  #./src/ggml-opt.cpp
  #./src/ggml-threading.cpp
)

#------------------------------------------------------------------------------
# GGML Demo

# ggmlapp
add_executable(ggmlapp
  #./simple/simple-backend.cpp
  #./simple/simple-ctx.cpp
  ./simple/foo.cpp

  ${GGML_SRC}
)
install(TARGETS ggmlapp
  RUNTIME DESTINATION ${CMAKE_BINARY_DIR}
  LIBRARY DESTINATION ${CMAKE_BINARY_DIR}
  ARCHIVE DESTINATION ${CMAKE_BINARY_DIR}
)

#------------------------------------------------------------------------------
