cmake_minimum_required(VERSION 3.13.0)
project(foo VERSION 1.0.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 11)

find_package(PkgConfig REQUIRED)
pkg_check_modules(NetCDF REQUIRED IMPORTED_TARGET netcdf>4.7.0)
pkg_check_modules(NetCDF-CXX4 REQUIRED IMPORTED_TARGET netcdf-cxx4>4.3.0)

add_executable(foo foo.cpp)

## using the following command will successfully 
## link with CMake 3.13.0
target_link_libraries(foo PkgConfig::NetCDF PkgConfig::NetCDF-CXX4)

## using the following commands will link 
## successfully with CMake 3.00.0 {
#target_link_libraries(foo 
#  ${NetCDF-CXX4_LDFLAGS}
#  ${NetCDF_LDFLAGS})
#target_include_directories(foo PUBLIC 
#  ${NetCDF-CXX4_INCLUDE_DIRS}
#  ${NetCDF_INCLUDE_DIRS})
#}
