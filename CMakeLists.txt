cmake_minimum_required(VERSION 2.8)
project(mpaligner)

# not used for now
set(PROJECT_VER_MAJOR 0)
set(PROJECT_VER_MINOR 9)
set(PROJECT_VER_PATCH 7)
set(PROJECT_VER "${PROJECT_VER_MAJOR}.${PROJECT_VER_MINOR}.${PROJECT_VER_PATCH}")
set(PROJECT_APIVER "${PROJECT_VER_MAJOR}.${PROJECT_VER_MINOR}")

# set the default path for built executables to the "bin" directory
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR}/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR}/lib)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR}/lib)

set(CURSES_NEED_NCURSES TRUE)
find_package(Curses)

option(WITH_OMP "compile mpaligner with openmp." OFF)

set(CMAKE_CFLAGS "${CMAKE_C_FLAGS} -O2")

if (WITH_OMP)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fopenmp")
endif()

file(GLOB SOURCES src/*.c)
file(GLOB INCLUDES src/*.h)

include_directories("src")
include_directories(${CURSES_INCLUDE_DIRS})

add_executable(mpaligner ${SOURCES} ${INCLUDES})
target_link_libraries(mpaligner m progressbar statusbar ${CURSES_LIBRARIES})
install(TARGETS mpaligner DESTINATION lib RUNTIME DESTINATION bin)
