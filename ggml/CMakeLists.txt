# Copyright 2025 <chaishushan{AT}gmail.com>. All rights reserved.
# Use of this source code is governed by a BSD-style
# license that can be found in the LICENSE file.

cmake_minimum_required (VERSION 3.30.0)

project(GGML)

set(CMAKE_CXX_STANDARD 17)

#------------------------------------------------------------------------------

# Extra flags to enable experimental features and code
add_definitions(
  #-DWEBP_HAVE_AVX2
)

include_directories(AFTER
  ./include
  ./src
  ./src/ggml-cpu
)

set(GGML_SRC
  ./src/ggml.c
  ./src/ggml-alloc.c
  ./src/ggml-backend.cpp
  ./src/ggml-backend-reg.cpp
  ./src/ggml-opt.cpp
  ./src/ggml-quants.c
  ./src/ggml-threading.cpp

  ./src/ggml-cpu/ggml-cpu.c
  ./src/ggml-cpu/ggml-cpu.cpp
  ./src/ggml-cpu/ggml-cpu-traits.cpp
  ./src/ggml-cpu/ggml-cpu-quants.c
)

#------------------------------------------------------------------------------
# GGML Demo

add_executable(simple-backend
  ./simple/simple-backend.cpp
  ${GGML_SRC}
)

add_executable(simple-ctx
  ./simple/simple-ctx.cpp
  ${GGML_SRC}
)

#------------------------------------------------------------------------------
