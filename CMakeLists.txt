cmake_minimum_required(VERSION 3.6)
project(nozip VERSION 1.0.0 LANGUAGES C)

set(CMAKE_C_STANDARD 99)

if(CMAKE_C_COMPILER_ID MATCHES "GNU|Clang")
  add_compile_options(-Wall -Wextra -Wno-unused-parameter)
elseif (CMAKE_C_COMPILER_ID MATCHES "MSVC")
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -D_CRT_SECURE_NO_WARNINGS")
endif()

add_library(libnozip STATIC src/nozip.c src/nozip.h)
add_library(nozip::libnozip ALIAS libnozip)
set_target_properties(libnozip PROPERTIES OUTPUT_NAME "unzip")
target_include_directories(libnozip PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/src")
add_executable(unzip src/unzip.c)
target_link_libraries(unzip nozip::libnozip)
