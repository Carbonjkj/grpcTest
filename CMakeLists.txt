
cmake_minimum_required(VERSION 3.15)
set( CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin )
set( CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib )
set(CMAKE_CXX_STANDARD 17)
set(PROTOBUF_INCLUDE_DIRS /home/kaijun/opt/include)
include_directories(include)
project(mygRPC)
add_subdirectory (src)