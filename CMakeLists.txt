cmake_minimum_required(VERSION 3.20)
project(rect)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(CMAKE_EXPORT_COMPILE_COMMANDS on)

# https://packages.msys2.org/package/mingw-w64-x86_64-opencv
# pacman -S mingw-w64-x86_64-opencv
# in `MSYS2 MinGW x64` if you want

if(DEFINED OpenCV_DIR)
    message("OpenCV_DIR: ${OpenCV_DIR}")
    find_package(OpenCV REQUIRED PATHS ${OpenCV_DIR})
else()
    find_package(OpenCV REQUIRED)
endif()

if (NOT DEFINED CLI11_DIR)
    set(CLI11_DIR "components/CLI11" CACHE STRING "CLI11 git repository")
endif ()
include_directories(${CLI11_DIR}/include)

option(USE_SYSTEM_SPDLOG "Use system spdlog" OFF)
if (USE_SYSTEM_SPDLOG)
    find_package(spdlog REQUIRED)
else()
    add_subdirectory(components/spdlog)
endif ()

add_executable(demo main.cpp)
target_link_libraries(demo PRIVATE ${OpenCV_LIBS} spdlog::spdlog)
