cmake_minimum_required(VERSION 3.27)
project(parx VERSION 0.0.1 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

add_subdirectory(engine)
add_subdirectory(sandbox)
add_subdirectory(tests)
