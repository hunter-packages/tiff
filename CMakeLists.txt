cmake_minimum_required(VERSION 3.0)

include("cmake/HunterGate.cmake")
HunterGate(
    URL "https://github.com/ruslo/hunter/archive/v0.8.1.tar.gz"
    SHA1 "a9bd96ab113f3af870b13cd05cf4e75b8848cf4a"
)

project(TIFF)

add_subdirectory(libtiff)
