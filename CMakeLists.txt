cmake_minimum_required(VERSION 3.0)
project(TIFF)

include("cmake/HunterGate.cmake")

HunterGate(
    URL "https://github.com/ruslo/hunter/archive/v0.7.0.tar.gz"
    SHA1 "e730118c7ec65126398f8d4f09daf9366791ede0"
)

add_subdirectory(libtiff)
