cmake_minimum_required(VERSION 3.4.1)

set(CMAKE_VERBOSE_MAKEFILE on)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wno-multichar -D_ANDROID  -DBUILDING_LIBICONV -DBUILDING_LIBCHARSET -DIN_LIBRARY")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/so/${ANDROID_ABI})
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

add_definitions(-DLIBDIR="c")

add_library(
    zbar

    SHARED

    src/main/jni/zbar/config.c
    src/main/jni/zbar/convert.c
    src/main/jni/zbar/decoder.c
    src/main/jni/zbar/error.c
    src/main/jni/zbar/image.c
    src/main/jni/zbar/img_scanner.c
    src/main/jni/zbar/refcnt.c
    src/main/jni/zbar/scanner.c
    src/main/jni/zbar/symbol.c
    src/main/jni/zbar/decoder/codabar.c
    src/main/jni/zbar/decoder/code39.c
    src/main/jni/zbar/decoder/code93.c
    src/main/jni/zbar/decoder/code128.c
    src/main/jni/zbar/decoder/databar.c
    src/main/jni/zbar/decoder/ean.c
    src/main/jni/zbar/decoder/i25.c
    src/main/jni/zbar/decoder/qr_finder.c
    src/main/jni/zbar/qrcode/bch15_5.c
    src/main/jni/zbar/qrcode/binarize.c
    src/main/jni/zbar/qrcode/isaac.c
    src/main/jni/zbar/qrcode/qrdec.c
    src/main/jni/zbar/qrcode/qrdectxt.c
    src/main/jni/zbar/qrcode/rs.c
    src/main/jni/zbar/qrcode/util.c
)

find_library(log-lib
             log)

include_directories(
    src/main/jni/include
    src/main/jni/zbar
    src/main/jni/iconv/include
)

set(libs "${CMAKE_SOURCE_DIR}/src/main/jni")

add_library(iconv SHARED IMPORTED)
set_target_properties(iconv PROPERTIES
    IMPORTED_LOCATION "${libs}/iconv/lib/${ANDROID_ABI}/libiconv.so")

target_link_libraries(
    zbar

    iconv
    ${log-lib}
)