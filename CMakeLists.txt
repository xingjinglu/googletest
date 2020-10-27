# Note: CMake support is community-based. The maintainers do not use CMake
# internally.

cmake_minimum_required(VERSION 2.8.8)

if (POLICY CMP0048)
  cmake_policy(SET CMP0048 NEW)
endif (POLICY CMP0048)

project(googletest-distribution)
set(GOOGLETEST_VERSION 1.10.0)

if (CMAKE_VERSION VERSION_GREATER "3.0.2")
  if(NOT CYGWIN AND NOT MSYS AND NOT ${CMAKE_SYSTEM_NAME} STREQUAL QNX)
    set(CMAKE_CXX_EXTENSIONS OFF)
  endif()
endif()

enable_testing()

include(CMakeDependentOption)
include(GNUInstallDirs)

#Note that googlemock target already builds googletest
option(BUILD_GMOCK "Builds the googlemock subproject" ON)
option(INSTALL_GTEST "Enable installation of googletest. (Projects embedding googletest may want to turn this OFF.)" ON)


############# Set cross compiler for arm ############
if(OPT_ARCH STREQUAL "arm64")
  set(arch "aarch64")
elseif(OPT_ARCH STREQUAL "arm32")
  set(arch "armv7")
elseif(OPT_ARCH STREQUAL "x86_64")
  set(arch "x86")
elseif(OPT_ARCH STREQUAL "x86_32")
  set(arch "x86")
elseif(OPT_ARCH STREQUAL "c")
  set(arch "x86")
endif()

if(OPT_ARCH STREQUAL "arm64")
  set(CMAKE_SYSTEM_NAME Linux)
  set(CMAKE_SYSTEM_PROCESSOR arm)
  set(tools ${ANDROID_NDK})
  set(CMAKE_C_COMPILER ${tools}/toolchains/llvm/prebuilt/linux-x86_64/bin/aarch64-linux-android21-clang)
  set(CMAKE_CXX_COMPILER ${tools}/toolchains/llvm/prebuilt/linux-x86_64/bin/aarch64-linux-android21-clang++)
elseif(OPT_ARCH STREQUAL "arm32")
  set(CMAKE_SYSTEM_NAME Linux)
  set(CMAKE_SYSTEM_PROCESSOR arm)
  set(tools ${ANDROID_NDK})
  set(CMAKE_C_COMPILER ${tools}/toolchains/llvm/prebuilt/linux-x86_64/bin/armv7a-linux-androideabi21-clang)
  set(CMAKE_C_COMPILER ${tools}/toolchains/llvm/prebuilt/linux-x86_64/bin/armv7a-linux-androideabi21-clang++)
endif()

if(BUILD_GMOCK)
  add_subdirectory( googlemock )
else()
  add_subdirectory( googletest )
endif()
