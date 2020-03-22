Linear Methods for Image Interpolation
======================================

Link to original work: http://www.ipol.im/pub/art/2011/g_lmii/revisions/2011-09-27/g_lmii.html  
In this repo added cmake to original sources "interp-src"  

Build
=====

Prerequisites
  * git
  * cmake
  * C compiler

Windows
-------

On windows we will use Microsoft package manager to build third-party libraries

  1. Install vcpkg and integrate with Visual Studio
  
    git clone https://github.com/microsoft/vcpkg
    cd vcpkg
    bootstrap-vcpkg.bat
    vcpkg integrate install
    
   Remember hint "CMake projects should use: -DCMAKE_TOOLCHAIN_FILE=Path/To/vcpkg.cmake"

  2. Install packages (static versions)
  
    vcpkg install fftw3:x64-windows-static
    vcpkg install libpng:x64-windows-static
    vcpkg install libjpeg-turbo:x64-windows-static
    vcpkg install tiff:x64-windows-static

  3. Build project

    mkdir build
    pushd build
    cmake -G "Visual Studio 15" -A x64 ^
      -DVCPKG_TARGET_TRIPLET=x64-windows-static ^
      -DCMAKE_TOOLCHAIN_FILE=Path/To/vcpkg.cmake ^
      ..
    cmake --build . --config Release
    popd
