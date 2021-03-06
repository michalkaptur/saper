project(minesweeper)
cmake_minimum_required(VERSION 3.1)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++14") #CXX_STANDARD variable does not work with clang

if ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "Clang")
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -stdlib=libc++")
	set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -lc++ -lc++abi -lm -lc -lgcc_s -lgcc")
endif()

file(GLOB_RECURSE CLI_SRC  ${CMAKE_SOURCE_DIR}/src/cli/*.cpp ${CMAKE_SOURCE_DIR}/src/cli/*.h)
file(GLOB_RECURSE LIB_SRC  ${CMAKE_SOURCE_DIR}/src/engine/*.cpp ${CMAKE_SOURCE_DIR}/src/engine/*.h)
file(GLOB_RECURSE TEST_SRC ${CMAKE_SOURCE_DIR}/test/*.cpp ${CMAKE_SOURCE_DIR}/test/*.h)
include_directories(${CMAKE_SOURCE_DIR}/src/)
add_library(minesweeperengine STATIC ${LIB_SRC})

add_executable(minesweeper ${CLI_SRC})
target_link_libraries(minesweeper minesweeperengine)

add_executable(ut ${TEST_SRC} ${LIB_SRC})
target_include_directories(ut PRIVATE
    $<BUILD_INTERFACE:${CMAKE_SOURCE_DIR}/external>
    $<BUILD_INTERFACE:${CMAKE_SOURCE_DIR}/src>
)
