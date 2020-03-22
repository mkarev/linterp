Linear Methods for Image Interpolation
======================================

Link to original work: http://www.ipol.im/pub/art/2011/g_lmii/revisions/2011-09-27/g_lmii.html  
This repo add cmake support to original project [src.zip](http://www.ipol.im/pub/art/2011/g_lmii/src.zip)

Build
=====

Prerequisites
  * git
  * cmake
  * C compiler

Windows
-------

On windows we will use Microsoft package manager to build third-party libraries  
**TODO**: add TIFF support

  1. Install vcpkg (without telemetry) and integrate it with Visual Studio
  
    git clone https://github.com/microsoft/vcpkg
    cd vcpkg
    bootstrap-vcpkg.bat -disableMetrics
    vcpkg integrate install
    
   Remember path to vcpkg toolchain file: "CMake projects should use: `-DCMAKE_TOOLCHAIN_FILE=Path/To/vcpkg.cmake`"  
   We will use it in step 3.

  2. Install packages (static versions)
  
    vcpkg install fftw3:x64-windows-static
    vcpkg install libpng:x64-windows-static
    vcpkg install libjpeg-turbo:x64-windows-static
    vcpkg install tiff:x64-windows-static

  3. Build project (change cmake generator to appropriate Visual Studio version if need)

    mkdir build
    pushd build
    cmake -G "Visual Studio 16 2019" -A x64 ^
      -DVCPKG_TARGET_TRIPLET=x64-windows-static ^
      -DCMAKE_TOOLCHAIN_FILE=Path/To/vcpkg.cmake ^
      ..
    cmake --build . --config Release
    popd
