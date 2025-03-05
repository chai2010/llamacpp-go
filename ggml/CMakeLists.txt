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
  ./include/webp.h
  ./src/webp.c

  ${WEBP_SRC}
)
if(CMAKE_BUILD_TYPE STREQUAL "debug")
  set_target_properties(webplib
    PROPERTIES OUTPUT_NAME "webp-${OS}-debug"
  )
else()
  set_target_properties(webplib
    PROPERTIES OUTPUT_NAME "webp-${OS}"
  )
endif()

install(TARGETS webplib
  RUNTIME DESTINATION ${CMAKE_CURRENT_SOURCE_DIR}
  LIBRARY DESTINATION ${CMAKE_CURRENT_SOURCE_DIR}
  ARCHIVE DESTINATION ${CMAKE_CURRENT_SOURCE_DIR}
)

#------------------------------------------------------------------------------
# WebP Demo

include_directories(AFTER
  ./webplib
)

# webpapp
add_executable(webpapp
  ./demo/demo.cc

  ./include/webp.h
  ./src/webp.c

  ${WEBP_SRC}
)
install(TARGETS webpapp
  RUNTIME DESTINATION ${CMAKE_CURRENT_SOURCE_DIR}
  LIBRARY DESTINATION ${CMAKE_CURRENT_SOURCE_DIR}
  ARCHIVE DESTINATION ${CMAKE_CURRENT_SOURCE_DIR}
)

#------------------------------------------------------------------------------
# test

include_directories(AFTER
  ./test
)

add_executable(webptest
  ./test/test.cc
  ./test/test_util.cc
  ./test/test_util_jpg.cc

  #./test/webp_test.cc
  #./test/webp_bench_test.cc

  ./include/webp.h
  ./src/webp.c

  ${WEBP_SRC}
)

install(TARGETS webptest
  RUNTIME DESTINATION ${CMAKE_CURRENT_SOURCE_DIR}
  LIBRARY DESTINATION ${CMAKE_CURRENT_SOURCE_DIR}
  ARCHIVE DESTINATION ${CMAKE_CURRENT_SOURCE_DIR}
)

#------------------------------------------------------------------------------
