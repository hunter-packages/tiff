cmake_minimum_required(VERSION 3.0)

include("cmake/HunterGate.cmake")
HunterGate(
    URL "https://github.com/ruslo/hunter/archive/v0.20.27.tar.gz"
    SHA1 "aafefc6377dc9a25d84703702e22434cd6f71d5c"
)
project(TIFF VERSION 4.0.2)

if(MSVC)
  add_definitions(-D_CRT_SECURE_NO_WARNINGS)
endif()

add_subdirectory(libtiff)
