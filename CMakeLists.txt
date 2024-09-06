cmake_minimum_required(VERSION 3.28)
project(Snake3D)

set(CMAKE_CXX_STANDARD 23)
set(path_engine ../WurstbudeEngine)
set(path_engine_build ${path_engine}/cmake-build-debug)

add_subdirectory(${path_engine} ${path_engine_build})

find_library(wurstbudeLib NAMES WurstbudeEngine PATHS ${path_engine_build} REQUIRED)

get_filename_component(wurstbudeLib_DIR ${wurstbudeLib} DIRECTORY)
message(STATUS "engine found at: ${wurstbudeLib_DIR}")

add_executable(Snake3D main.cpp)
target_include_directories(Snake3D PUBLIC ${path_engine})
target_link_libraries(Snake3D ${wurstbudeLib})

add_custom_command(TARGET Snake3D POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy
        ${wurstbudeLib_DIR}/libWurstbudeEngine.dll
        $<TARGET_FILE_DIR:Snake3D>)
