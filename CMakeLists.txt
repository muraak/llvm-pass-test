
set(LLVM_DIR /usr/local/Cellar/llvm/7.0.1/share/cmake/modules)
set(LLVM_INCLUDE_DIRS /usr/local/Cellar/llvm/7.0.1/include)

cmake_minimum_required(VERSION 3.1)

find_package(LLVM REQUIRED CONFIG)
add_definitions(${LLVM_DEFINITIONS})
include_directories(${LLVM_INCLUDE_DIRS})
link_directories(${LLVM_LIBRARY_DIRS})

add_subdirectory(Pass)  # Use your pass name here.
