# Build libraries for GSI from the [NCEPLIBS](https://github.com/NOAA-EMC/NCEPLIBS) repository

**The [UFS support forum](https://forums.ufscommunity.org) provides helps on how to build NCEPLIBS. Please submit your questions there.**

The following is just to share our experiences on building parts of NCEPLIBS to meet the GSI requirement.
*This method may avoid the need to compile NCEPLIBS-external if your system already has the following libraries installed: MPI zlib HDF5 NetCDF4 libpng*

### 1. Clone NCEPLIBS from Github
```
git clone -b ufs-v1.0.0 --recursive https://github.com/NOAA-EMC/NCEPLIBS
cd NCEPLIBS
```

### 2. Modify CMakeLists.txt
  Make sure the following 8 lines are commented out in the CMakeLists.txt
```
#add_subdirectory(NCEPLIBS-g2)
...
#add_subdirectory(NCEPLIBS-bufr)
#add_subdirectory(NCEPLIBS-g2tmpl)
...
#add_subdirectory(NCEPLIBS-landsfcutil)
....
#add_subdirectory(NCEPLIBS-grib_util)
#add_subdirectory(NCEPLIBS-prod_util)
#add_subdirectory(UFS_UTILS)
#add_subdirectory(NCEPLIBS-post)
```

### 3. Build NCEPLIBS and install libraries

##### 3.1 Prepare the building environment
You will need to load appropriate modules (or setting up correct environmental variables) required by the build process.

For example, here is what we used at one computing platform:
```
1) intel/18.0.5   2) ncarenv/1.3   3) ncarcompilers/0.5.0   4) impi/2018.4.274
5) mkl/2018.0.5   6) netcdf/4.7.3  7) cmake/3.16.4
```

##### 3.2 Set the environment variables CC, CXX and FC
Here is an example:
```
setenv CC icc; setenv CXX icpc; setenv FC ifort
```

##### 3.3 Build...
```
 mkdir build;  cd build
 cmake ..
 make -j8
```

##### 3.4 Install...
```
 make install
```

### 4. make links

The compiled libraries are under the `build/install/lib` directory and each file has version numbers in the filename (see below). These version numbers may change with different NCEPLIBS releases.

The build.comgsi script tries to find files without version numbers in their names, as follows:
```
export W3NCO_LIBd=${NCEPLIBS}/lib/libw3nco_d.a
export W3NCO_LIB4=${NCEPLIBS}/lib/libw3nco_4.a
```

The "linklibs" script in the GSILIBS repo can remove version numbers in the filename for all libraries.

Assuming GSILIBS/ and NCEPLIBS/ are under the same parent directory:
```
cd build/install/lib
../../../../GSILIBS/linklibs
```

Now you should have something as follows:
```
 libw3nco_4.a -> ./libw3nco_v2.1.0_4.a
 libw3nco_8.a -> ./libw3nco_v2.1.0_8.a
 libw3nco_d.a -> ./libw3nco_v2.1.0_d.a
```



