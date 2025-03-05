# Copyright 2025 <chaishushan{AT}gmail.com>. All rights reserved.
# Use of this source code is governed by a BSD-style
# license that can be found in the LICENSE file.

project(GGML)

#------------------------------------------------------------------------------

IF(WIN32)
  if(CMAKE_SIZEOF_VOID_P EQUAL 8)
    set(OS win64)
  else()
    set(OS win32)
  endif()
else()
  if(CMAKE_SIZEOF_VOID_P EQUAL 8)
    set(OS posix64)
  else()
    set(OS posix64)
  endif()
endif()

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
  ./src/ggml-alloc.c
  ./src/ggml-quants.c
  ./src/ggml.c

  ./src/ggml-backend-reg.cpp
  ./src/ggml-backend.cpp
  ./src/ggml-opt.cpp
  ./src/ggml-threading.cpp
)

#------------------------------------------------------------------------------

add_library(ggmllib STATIC
  ${GGML_SRC}
)
if(CMAKE_BUILD_TYPE STREQUAL "debug")
  set_target_properties(ggmllib
    PROPERTIES OUTPUT_NAME "ggml-${OS}-debug"
  )
else()
  set_target_properties(ggmllib
    PROPERTIES OUTPUT_NAME "ggml-${OS}"
  )
endif()

install(TARGETS ggmllib
  RUNTIME DESTINATION ${CMAKE_CURRENT_SOURCE_DIR}
  LIBRARY DESTINATION ${CMAKE_CURRENT_SOURCE_DIR}
  ARCHIVE DESTINATION ${CMAKE_CURRENT_SOURCE_DIR}
)

#------------------------------------------------------------------------------
# GGML Demo

# ggmlapp
add_executable(ggmlapp
  ./simple/simple-backend.cpp
  ./simple/simple-ctx.cpp

  ${GGML_SRC}
)
install(TARGETS ggmlapp
  RUNTIME DESTINATION ${CMAKE_CURRENT_SOURCE_DIR}
  LIBRARY DESTINATION ${CMAKE_CURRENT_SOURCE_DIR}
  ARCHIVE DESTINATION ${CMAKE_CURRENT_SOURCE_DIR}
)

#------------------------------------------------------------------------------
