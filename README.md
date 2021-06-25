# How to build community GSI/EnKF?

# 1. Hera, Jet, Orion, Cheyenne users:
For users on the following supercomputers `Hera, Jet, Orion, Cheyenne`, the build process is very straight forward:
```
git clone https://github.com/NOAA-EMC/GSI
cd GSI
ush/build.comgsi
```
The build.comgsi script will load required modules, set required environmental variables and build GSI, regional EnKF and other utilities. All executables are under the build/bin directory.

# 2. Community users:

With the transition of NOAA GSI/EnKF codes to Github, the original (outdated) libsrc/ was no longer used and those libraries will come from the [hpc-stack](http://github.com/NOAA-EMC/hpc-stack/) repository.

Community users can follow these steps to build GSI/EnKF:

## 2.1. build required libraries from the hpc-stack

The NOAA hpc-stack repository is at http://github.com/NOAA-EMC/hpc-stack/. You can follow instructions there to build required libraries.

An example to build comgsi-flavored hpc-stack:
```
git clone https://github.com/NOAA-EMC/hpc-stack git.hpc_stack
cd git.hpc-stack
(... load comipiler, mpi and netcdf modules ...)
./setup_modules.sh -p /path/to/hpc-stacks -c config/config_comgsi.sh
./build_stack.sh -m -p /path/to/hpc-stacks  -c config/config_comgsi.sh -y config/stack_comgsi.yaml
```

The above commands were tested on the commit `c566b943fc6ac648fd884c93e2ac86a181686e9d` as of May 26, 2021.

## 2.2. build GSI/EnKF

#### (1) Clone the GSI repository:
```
git clone https://github.com/NOAA-EMC/GSI
```

#### (2) load hpc-stack modules following the following example (your hpc-intel, hpc-impi may have different version numbers)
Be sure to module load intel, impi and netcdf first
```
module use /path/to/hpc-stacks/modulefiles/stack
module load hpc/1.1.0
module load hpc-intel/18.0.5
module load hpc-impi/2018.4.274
module load bufr/11.5.0
module load ip/3.3.3
module load nemsio/2.5.2
module load sfcio/1.4.1
module load sigio/2.3.2
module load sp/2.3.3
module load w3nco/2.4.1
module load w3emc/2.7.3
module load bacio/2.4.1
module load crtm/2.3.0
module load wrf_io/1.2.0
```

#### (3) Compile
```
cd GSI
mkdir build; cd build
cmake -DCOMGSI=ON -DENKF_MODE=WRF ..
make -j4
```

## 2.4. fix files
You can get some basic fix files from the comgsi/fix repository:
```
git clone https://github.com/comgsi/fix
```
For a complete set of fix files, please download them from the [DTC website](https://dtcenter.org/community-code/gridpoint-statistical-interpolation-gsi/download). 

If you have any questions, please visit the GSI/EnKF user support forum: https://dtcenter.org/forum/gsi-enkf-user-support
