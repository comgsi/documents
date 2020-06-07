# How to build community GSI/EnKF?

# 1. Hera, Jet, Orion, Cheyenne users:
For users on the following supercomputers `Hera, Jet, Orion, Cheyenne`, the build process is very straight forward:
```
git clone https://github.com/comgsi/GSI
cd GSI
ush/build.comgsi
```
The build.comgsi script will load required modules, set required environmental varialbes and build GSI, regional EnKF and other utilities. All executables are under the build/bin directory.

# 2. Community users:

With the transition of NOAA GSI/EnKF codes to Github, the origial (outdated) libsrc/ was removed and those libraries will come from the [NCEPLIBS](https://github.com/NOAA-EMC/NCEPLIBS) repository released by [UFS](https://github.com/ufs-community/ufs-weather-model/wiki).  For how to build NCEPLIBS, please seek helps from the [UFS support forum](https://forums.ufscommunity.org).

Community users can follow these steps to build GSI/EnKF:

## 2.1. build bufrlib and wrfio lib
This extra step is required as NCEPLIBS does not release bufrlib and wrfio lib at this moment. In the future, when NCEPLIBS includes these two libraries, this step will not be needed.

You can build these two libraries from the [comgsi/GSILIBS](https://github.com/comgsi/GSILIBS) repo at Github as follows:
```
git clone https://github.com/comgsi/GSILIBS
cd GSILIBS
mkdir build; cd build
cmake -DBUILD_CORELIBS=ON ..
make -j8
```
The compiled libraries will be under the build/lib directory. You will need this path in step 2.3.3.

## 2.2 build NCEPLIBS
The [UFS support forum](https://forums.ufscommunity.org) provides helps on how to build NCEPLIBS. Please submit your questions there.

FYI, [Here is a quick summary](NCEPLIBS.md) how we installed NCEPLIBS.

## 2.3. build GSI/EnKF

#### (1) Clone the comgsi/GSI repo:
```
git clone https://github.com/comgsi/GSI
```

#### (2) create a file to list all modules required by the build process:
for example, a file `modulefile.me.GSI_UPP_WRF` may looks like as follows:
```
module purge
module load intel/18.0.5 ncarenv ncarcompilers
module load impi/2018.4.274
module load mkl
module load netcdf
module load cmake/3.16.4
```
#### (3) change the following three variables in the ush/build.comgsi script based on your computer enviroment
    modulefile="/my/modulefile.me.GSI_UPP_WRF"
    NCEPLIBS="/my/NCEPLIBS/b_intel18.0.5_impi2018.4.274/install"
    GSILIBS="/my/GSILIBS/b_intel18.0.5_impi2018.4.274/"

#### (4) run "ush/build.comgsi" from the main GSI directory

**NOTE** For those who don't have the envorimental module system in your computing platform, you may follow the ush/build.comgsi script on how to set up enviormental variables and then do the compling.

## 2.4. fix files
You can get some basic fix files from the comgsi/fix repository:
```
git clone https://github.com/comgsi/fix
```
For a complete set of fix files, please downlod them from the [DTC website](https://dtcenter.org/community-code/gridpoint-statistical-interpolation-gsi/download). 

If you have any questions, please contact gsi-help@ucar.edu. 
