cmake_minimum_required(VERSION 3.5)
project(libcamera)

set(libcamera_MAJOR_VERSION 0)
set(libcamera_MINOR_VERSION 0)
set(libcamera_PATCH_VERSION 0)
set(libcamera_VERSION
  ${libcamera_MAJOR_VERSION}.${libcamera_MINOR_VERSION}.${libcamera_PATCH_VERSION})

# set default build type
# https://blog.kitware.com/cmake-and-the-default-build-type/
if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
  set(default_build_type "Release")
  message(STATUS "Setting build type to '${default_build_type}' as none was specified.")
  set(CMAKE_BUILD_TYPE "${default_build_type}" CACHE
      STRING "Choose the type of build." FORCE)
  # Set the possible values of build type for cmake-gui
  set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS
    "Debug" "Release" "MinSizeRel" "RelWithDebInfo")
endif()

string(TOLOWER ${CMAKE_BUILD_TYPE} MESON_BUILD_TYPE)

find_package(GnuTLS REQUIRED)
find_package(OpenSSL REQUIRED)

include(ExternalProject)

ExternalProject_Add(libcamera_source
  GIT_REPOSITORY      https://git.libcamera.org/libcamera/libcamera.git
  CONFIGURE_COMMAND   meson build ../libcamera_source --prefix ${CMAKE_INSTALL_PREFIX} --libdir lib --buildtype ${MESON_BUILD_TYPE} -Dcam=disabled -Ddocumentation=disabled -Dgstreamer=disabled -Dlc-compliance=disabled -Dqcam=disabled -Dtest=false -Dtracing=disabled -Dv4l2=true
  BUILD_COMMAND       meson compile -C build
  INSTALL_COMMAND     meson install -C build
)

add_library(libcamera SHARED IMPORTED)
add_dependencies(libcamera libcamera_source)
