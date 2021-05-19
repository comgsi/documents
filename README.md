# How to build community GSI/EnKF?

# 1. Hera, Jet, Orion, Cheyenne users:
For users on the following supercomputers `Hera, Jet, Orion, Cheyenne`, the build process is very straight forward:
```
git clone https://github.com/comgsi/GSI (or any GSI code after 5/18/2021)
cd GSI
ush/build.comgsi
```
The build.comgsi script will load required modules, set required environmental variables and build GSI, regional EnKF and other utilities. All executables are under the build/bin directory.

# 2. Community users:

With the transition of NOAA GSI/EnKF codes to Github, the original (outdated) libsrc/ was no longer used and those libraries will come from the [hpc-stack](http://github.com/NOAA-EMC/hpc-stack/) repository.

Community users can follow these steps to build GSI/EnKF:

## 2.1. build required libraries from the hpc-stack

Becuase the wrf_io library in the hpc-stack does not compile GSI/EnKF, you are encouraged to clone the hpc-stack from the comgsi fork https://github.com/comgsi/hpc-stack where an alternate gsiwrfio library (https://github.com/comgsi/gsiwrfio) and customized config_comgsi.sh, stack_comgsi.yaml are provided for your convenience.

An example to build hpc-stack:
```
git clone https://github.com/comgsi/hpc-stack git.hpc_stack
cd git.hpc-stack
(... load comipiler, mpi and netcdf modules ...)
./setup_modules.sh -p ../hpc-stacks -c config/config_comgsi.sh
./build_stack.sh -m -p ../hpc-stacks  -c config/config_comgsi.sh -y config/stack_comgsi.yaml
```

Reference this document (https://github.com/comgsi/hpc-stack#readme) for more information.

## 2.2. build GSI/EnKF

#### (1) Clone the comgsi/GSI repository:
```
git clone https://github.com/comgsi/GSI
```

#### (2) load hpc-stack modules following this example (your hpc-intel, hpc-impi may have different version numbers)
```
module use /path/to/hpc-stacks/modulefiles/stack
module load hpc/1.1.0
module load hpc-intel/18.0.5
module load hpc-impi/2018.4.274
module load bufr/11.4.0
module load ip/3.3.3
module load nemsio/2.5.2
module load sfcio/1.4.1
module load sigio/2.3.2
module load sp/2.3.3
module load w3nco/2.4.1
module load w3emc/2.7.3
module load bacio/2.4.1
module load crtm/2.3.0
module load gsiwrfio/1.0.0
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
