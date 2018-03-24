cmake_minimum_required(VERSION 3.0)

include("cmake/HunterGate.cmake")
HunterGate(
    URL "https://github.com/ruslo/hunter/archive/v0.9.9.tar.gz"
    SHA1 "3f4283a229e990b7faa82dd6e10d8b71c3f2e409"
)
project(TIFF VERSION 4.0.2)

if(MSVC)
  add_definitions(-D_CRT_SECURE_NO_WARNINGS)
endif()

add_subdirectory(libtiff)
