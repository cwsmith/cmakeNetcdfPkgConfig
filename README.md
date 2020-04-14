# cmakeNetcdfPkgConfig
Demonstrates linking problem using CMake PkgConfig `pkg_check_modules` to import NetCDF
and NetCDF-CXX4 targets when using the wrong `cmake_minimum_version`.

Using `cmake_minimum_version(VERSION 3.00.0)` and imported targets from 
PkgConfig via
`target_link_libraries(foo PkgConfig::NetCDF PkgConfig::NetCDF-CXX4)`
results in the following linking error:

```
$ make 
Scanning dependencies of target foo
[ 50%] Building CXX object CMakeFiles/foo.dir/foo.cpp.o
[100%] Linking CXX executable foo
/gpfs/u/software/dcs-spack-install/v0133gccSpectrum/linux-rhel7-power9le/gcc-7.4.0-1/netcdf-cxx4-4.3.1-7lwefeghu3za7l46xhlezrgdrhgrddq4/lib/libnetcdf_c++4.so:
undefined reference to `nc_inq_var_filter'
/gpfs/u/software/dcs-spack-install/v0133gccSpectrum/linux-rhel7-power9le/gcc-7.4.0-1/netcdf-cxx4-4.3.1-7lwefeghu3za7l46xhlezrgdrhgrddq4/lib/libnetcdf_c++4.so:
undefined reference to `nc_def_var_filter'
collect2: error: ld returned 1 exit status
make[2]: *** [foo] Error 1
make[1]: *** [CMakeFiles/foo.dir/all] Error 2
```

Using `cmake_minimum_version(VERSION 3.13.0)` will result in successful
linking.

## contents

- CMakeLists.txt: build file
- foo.cpp: netcdfcxx source code
- LICENSE: legal stuff
- README.md: this file
- netcdfPkgConfig: directory with netcdf pkgconfig files
  - netcdf-cxx4.pc
  - netcdf.pc

## setup

Use spack to install netcdf and netcdf-cxx4.

```
git clone https://github.com/spack/spack.git
cd spack
git checkout v0.13.3
export SPACK_ROOT=$PWD/spack/
export PATH=$SPACK_ROOT/bin:$PATH
source $SPACK_ROOT/share/spack/setup-env.sh
#setup compilers and packages if needed
spack install netcdf-cxx4@4.3.1 ^netcdf@4.7.2+parallel-netcdf ^hdf5+hl+cxx
```

This will create the pkgconfig files located in the `netcdfPkgConfig` directory.

## build the example package

```
ncDir=`spack location -i netcdf@4.7.2`
ncxxDir=`spack location -i netcdf-cxx4@4.3.1`
export PKG_CONFIG_PATH=\
$PKG_CONFIG_PATH:\
${ncDir}/lib/pkgconfig:\
${ncxxDir}/lib/pkgconfig
# setup compiler (i.e., module load ...)
git clone https://github.com/cwsmith/cmakeNetcdfPkgConfig.git
mkdir build-cmakeNetcdfPkgConfig
cd !$
cmake ../cmakeNetcdfPkgConfig
make
```
