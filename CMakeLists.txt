cmake_minimum_required(VERSION 3.0)
project(TIFF)

include("cmake/HunterGate.cmake")

HunterGate(
    URL "https://github.com/ruslo/hunter/archive/v0.7.0-zlib.tar.gz"
    SHA1 "6fd76523024137921757eca3baf885a637934677"
)

add_subdirectory(libtiff)
