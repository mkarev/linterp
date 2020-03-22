cmake_minimum_required(VERSION 3.16)

project(interp)

enable_language(C CXX)
if (MSVC)
    set(COMMON_FLAGS          "/WX /MP /D_CRT_SECURE_NO_WARNINGS /DUNICODE /D_UNICODE") # /WX treat warnings as error (-Werror)
    set(CMAKE_C_FLAGS_DEBUG   "/MTd /Zi /Ob0 /Od /RTC1")                                # /MT,/MTd multithread, static run-time
    set(CMAKE_C_FLAGS_RELEASE "/MT  /Zi /O2 /Ob2 /Oi /Ot /DNDEBUG")                     # /Zi PDB debuginfo
    set(CMAKE_C_FLAGS_RELWITHDEBINFO "${CMAKE_C_FLAGS_RELEASE}")
else ()
    set(COMMON_FLAGS          "${COMMON_FLAGS} -Wall -Wextra -Werror -Wno-unused-parameter -Wshadow")
    set(CMAKE_C_FLAGS         "${CMAKE_C_FLAGS} -Wstrict-prototypes")
    set(CMAKE_C_FLAGS_DEBUG   "-O0 -g")
    set(CMAKE_C_FLAGS_RELEASE "-O3 -DNDEBUG")
    set(CMAKE_C_FLAGS_RELWITHDEBINFO "${CMAKE_C_FLAGS_RELEASE} -g")
endif ()
set(CMAKE_POSITION_INDEPENDENT_CODE ON)
set(CMAKE_C_STANDARD 99)
set(CMAKE_C_FLAGS                  "${CMAKE_C_FLAGS}   ${COMMON_FLAGS}")

find_package(FFTW3f CONFIG REQUIRED)
find_package(libpng CONFIG REQUIRED)
find_package(JPEG REQUIRED)

add_library(common STATIC
    basic.c
    basic.h
    imageio.c
    imageio.h
    strutil.c
    strutil.h
)
target_link_libraries(common PUBLIC FFTW3::fftw3f png_static ${JPEG_LIBRARIES})
target_compile_definitions(common PUBLIC -DLIBPNG_SUPPORT -DLIBJPEG_SUPPORT)
target_include_directories(common PUBLIC ${JPEG_INCLUDE_DIR})

add_executable(linterp
    linterp.c
    linterp.h
    linterpcli.c
    lkernels.c
    lkernels.h
    lprefilt.c
    lprefilt.h
    adaptlob.c
    adaptlob.h
)
target_link_libraries(linterp common)

add_executable(imcoarsen imcoarsen.c)
target_link_libraries(imcoarsen common)

add_executable(imdiff
    imdiff.c
    conv.c
    conv.h
)
target_link_libraries(imdiff common)
