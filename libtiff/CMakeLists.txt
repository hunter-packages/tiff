# ----------------------------------------------------------------------------
#  CMake file for libtiff. See root CMakeLists.txt
#
# ----------------------------------------------------------------------------
include(CheckFunctionExists)
include(CheckIncludeFile)
include(CheckLibraryExists)
include(TestBigEndian)

check_include_file(assert.h HAVE_ASSERT_H)
check_include_file(fcntl.h HAVE_FCNTL_H)
check_include_file(io.h HAVE_IO_H)
check_function_exists(jbg_newlen HAVE_JBG_NEWLEN)
check_function_exists(mmap HAVE_MMAP)
check_include_file(search.h HAVE_SEARCH_H)
check_include_file(string.h HAVE_STRING_H)
check_include_file(sys/types.h HAVE_SYS_TYPES_H)
check_include_file(unistd.h HAVE_UNISTD_H)

test_big_endian(WORDS_BIGENDIAN)

if(WIN32 AND NOT HAVE_WINRT)
  set(USE_WIN32_FILEIO 1)
endif()

configure_file(
    "${CMAKE_CURRENT_SOURCE_DIR}/tif_config.h.cmakein"
    "${CMAKE_CURRENT_BINARY_DIR}/tif_config.h"
    @ONLY
)

hunter_add_package(ZLIB)
find_package(ZLIB REQUIRED CONFIG)

include_directories(
    "${CMAKE_CURRENT_SOURCE_DIR}"
    "${CMAKE_CURRENT_BINARY_DIR}"
)

set(
    lib_srcs
    tif_aux.c
    tif_close.c
    tif_codec.c
    tif_color.c
    tif_compress.c
    tif_dir.c
    tif_dirinfo.c
    tif_dirread.c
    tif_dirwrite.c
    tif_dumpmode.c
    tif_error.c
    tif_extension.c
    tif_fax3.c
    tif_fax3sm.c
    tif_flush.c
    tif_getimage.c
    tif_jbig.c
    tif_jpeg_12.c
    tif_jpeg.c
    tif_luv.c
    tif_lzma.c
    tif_lzw.c
    tif_next.c
    tif_ojpeg.c
    tif_open.c
    tif_packbits.c
    tif_pixarlog.c
    tif_predict.c
    tif_print.c
    tif_read.c
    tif_strip.c
    tif_swab.c
    tif_thunder.c
    tif_tile.c
    tif_version.c
    tif_warning.c
    tif_write.c
    tif_zip.c
    tif_stream.cxx
    t4.h
    tif_dir.h
    tif_fax3.h
    tiff.h
    tiffio.h
    tiffiop.h
    tiffvers.h
    tif_predict.h
    uvcode.h
    tiffio.hxx
    "${CMAKE_CURRENT_BINARY_DIR}/tif_config.h"
)

if(WIN32 AND NOT HAVE_WINRT)
  list(APPEND lib_srcs tif_win32.c)
else()
  list(APPEND lib_srcs tif_unix.c)
endif()

add_library(libtiff ${lib_srcs})
target_link_libraries(libtiff PUBLIC ZLIB::zlib)

# Some platforms (e.g. Linux) need separate math library
check_library_exists(m pow "" LIB_M_REQUIRED)
if(LIB_M_REQUIRED)
  target_link_libraries(libtiff PRIVATE m)
endif()

if(UNIX AND CMAKE_COMPILER_IS_GNUCXX)
  set_target_properties(libtiff PROPERTIES POSITION_INDEPENDENT_CODE TRUE)
endif()

if(ENABLE_SOLUTION_FOLDERS)
  set_target_properties(libtiff PROPERTIES FOLDER "3rdparty")
endif()

install(TARGETS libtiff DESTINATION lib EXPORT TIFFTargets)
install(EXPORT TIFFTargets NAMESPACE TIFF:: DESTINATION lib/cmake/TIFF)
install(FILES TIFFConfig.cmake DESTINATION lib/cmake/TIFF)
install(
    FILES "${CMAKE_CURRENT_BINARY_DIR}/tif_config.h" tiffio.h tiff.h tiffvers.h
    DESTINATION include
)

target_include_directories(libtiff PUBLIC "$<INSTALL_INTERFACE:include>")
