% I/O analysis of climate applications 
% Arne Beer, MN 6489196, Frank Röder, MN 6526113

\pagebreak

#Introduction
## About the paper

In this paper we analyse and present the pros and cons of different data structures required by a some carefully picked climate and weather prediction models.
Further we look at the absolute bare minimum of data required by those models.

## Getting started
With intent to get an overview about the richness of climate,land,ice, weather and ocean models we took a look at some in depth to work out that the approachability and documentation was not that clear.
Tons of very old models passed our way of searching through the sides of got dusty projects and source code. The question than was to get an good overview of up to date and easy to handle models which are still supported and updated.

## How we want to help
bla bla bla jeder kann sich an dem Paper schnell und effektive bedienen

# IFS - Integrated Forecasting System

## About IFS
IFS is a Model by European Center for Medium-range Weather Forecast (ECMWF) which is used to make analysis of data. This data can be a variety of different physical bulks.
This model looked quite promising as they offered an OpenIFS version of the model.
After some research we discovered that the license forbids "Commercial and benchmarking use of OpenIFS models", which stopped us from further investigation.
I would recommend to use this model in a research or academic context, as there is plenty of documentation and a big user base.


# Unidata - Awips2

AWIPS2 is a package which contains weather forecast display and analysis. This open-source `Java` application consists of `EDEX` a data server and CAVE the client for data analysis and rendering. 

## Installation
For the installation of `Awips2` ones can easily download the repository from Github and make it run with `installCave.sh` and `installEDEX.sh`. Those install scripts use yum as a package manager are currently supported for CentOS, Fedora and RedHead.
To make it compatible for the cluster there is maybe more to be done. Awips2 is normally installed with the help of the package manger YUM which could lead to some problems if you' re not the root. 
Awips2 requires a directory at root location "/awips2/". There are about 2000 lines of code where "/awips2/" is hard-coded, so switching directories is not an option.

**To build** a version for our purpose it would be the best to have a EDEX on the cluster which is providing our local CAVE with data for visualization.

# CESM - Community Earth System Model

## About CESM
CESM itself consists of seven geophysical models like ocean, land, ice, atmosphere ... . The CESM project is made and supported by U.S. climate researchers and mainly by the National Science Foundation (NSF).
The scientific development is conducted by the CESM working group twice a year. For more information related to the development its recommended to visit the website [link to source].

## Requirements
Here are some preconditions directly taken from the documentation of CESM.
In favour to make it run on the cluster we are working with we have to walk through the list:

* UNIX style operating system such as CNL, AIX and Linux        \checkmark
* csh, sh, and perl scripting languages                \checkmark
* subversion client version 1.4.2 or greater        \checkmark
* Fortran (2003 recommended, 90 required) and C compilers. pgi, intel, and xlf are recommended compilers.        \checkmark (gfortran gcc-Version 4.8)
* MPI (although CESM does not absolutely require it for running on one processor)        \checkmark
* NetCDF 4.2.0 or newer.        \checkmark (Version 7.3 & 4.2)
* ESMF 5.2.0 or newer (optional).
* pnetcdf 1.2.0 is required and 1.3.1 is recommended        ??? 
* Trilinos may be required for certain configurations        X
* LAPACKm or a vendor supplied equivalent may also be required for some configurations.                \checkmark (Version 3.0)
* CMake 2.8.6 or newer is required for configurations that include CISM.        \checkmark (Version 2.8.12.2)

## Quickstart
This Quickstart should give an overview about the work flow of CESM especially when there is already a version ported to the local target machine. If there is nothing already ported on the start machine, start with the more detailed description below.

There are a couple of definitions which must be unterstand and lead you through this section

        $COMPSET refers to the components set
        $RES refers to the model resolution
        $MACH refers to the target machine
        $CCSMROOT refers to the CESM root directory
        $CASE refers to the case name
        $CASEROOT refers to the full pathname of the root directory where the case ($CASE) will be created
        $EXEROOT refers to the executable directory ($EXEROOT is normally __not__ the same as $CASEROOT)
        $RUNDIR refers to the directory where CESM actually runs. This is normally set to $EXEROOT/run. (changing $EXEROOT does not change $RUNDIR as these are independent variavles)

As first step you need to [download(Link)](http://www.cesm.ucar.edu/models/cesm1.2/cesm/doc/usersguide/x290.html) CESM and select a machine, a component set and a resolution form the list displayed after using this commands:
        
        > cd $CCSMROOT/scripts
        > create_newcase -list
There is a list CESM supported CESM components like [sets(Link)](http://www.cesm.ucar.edu/models/cesm1.2/cesm/doc/modelnl/compsets.html), [resolution(Link)](http://www.cesm.ucar.edu/models/cesm1.2/cesm/doc/modelnl/grid.html) and [machines(Link)](http://www.cesm.ucar.edu/models/cesm1.2/cesm/doc/modelnl/machines.html).
To create a case the command `create_newcase` is used. It creates a case directory containing the scripts and XML files to set up the configurations for resolution, component set and machine requested. The `create_newcase` has some arguments as condition and some additional options for generic machines. For more information `create_newcase -h` should help.
I case that a supported machine is in use `($MACH)` tipe the following words:

        >create_newcase -case $CASEROOT \
                -mach $MACH \
                -compset $COMPSET \
                -res $RES

For running a new target machine use the __section below__.

To setup the case run script be sure to use the `cesm_setup` command which creates a $CASEROOT/$CASE.run script with `user_nl_xxx` files, while the xxx tell us something about the case configuration. But before running `cesm_setup` there is the `env_mach_pes.xml` file in $CASEROOT to be modified fitting to the experiment to be run.
        
        > cd $CASEROOT

After this the `env_mach_pes.xml` can be modified with the ___xmlchange___ command. Take a look at `xmlchange -h` for detailed information. Then the `cesm_setup` can be initiated.

        > ./cesm_setup

With the optional build modifications in mind (`env_mach_pes.xml`) the build script can be startet:

        > $CASE.build

To run the case and maybe setting the variable $DOUT_S in `env_mach_pes.xml` to `false` the job can be submitted to the batch queue:

        > $CASE.submit

After the job finished you can review all the following directories and files like:

        1. $RUNDIR
          * the directory set in the `env_build.xml` file
          * the location where the CESM was run with log files for every part
        2. $CASEROOT/logs
          * if the run was successful the log files have been copied into this directory
        3. $CASEROOT
          * here should a standard out or error file
        4. CASEROOT/CaseDocs
          * a list a case names is copied to this directory
        5. CASEROOT/timing
          * here are timing files which are representing the performance of the model
        6. $DOUTS_S_ROOT/$CASE
          * This directory is an archive depending on the setting done above, while it is true there is a log and history

## Installation
- Open source
- Download at [CCMS(Link)](http://www.cesm.ucar.edu/models/cesm1.2/cesm/doc/usersguide/x290.html#download_ccsm_code):
    - Username: guestuser
    - Password: friendly
- Version 1.2.1
- Available with svn: 

        svn co https://svn-ccsm-models.cgd.ucar.edu/cesm1/release_tags/cesm1_2_1 \
        cesm1_2_1 --username guestuser --password friendly

- We recommend to create an entry in your `~/.subversion/servers` config for later svn usage with scripts:

        [groups]
        cesm = svn-ccsm-inputdata.cgd.ucar.edu

        [cesm]
        username = guestuser
        store-passwords = yes

Most parts of the CESM software project are open source. However three libraries are published by the Los Almos National Laboratory, who licensed their software as free to use as long as it isn't used in a commercial context. Affected libraries are POP, SCRI and CICE [(Link to licence)](http://www.cesm.ucar.edu/management/UofCAcopyright.ccsm3.html).

## Input Data Set
### Setup
There is actually a set of input data which can be downloaded and configured for CESM. It can be made available through another Subversion input data repository by using the same user name as used in the installation above.
The dataset is around 1 TB big and should not be downloaded at ones. The download is regulated on demand, so if CESM needs the particular data it will be downloaded and checked automatically be CESM itself. The data should be on a disk in the local area.
The CESM variable `$DIN_LOCK_ROOT` has to be set inside of the script. Multiple users can use the same `$DIN_LOCK_ROOT` directory and should be configured as group writable.
If the machine is supported there is a preset otherwise there is a possibility to make it also run on generic machines with the variable as argument for the `./scripts/create_newcase` script .
Files in the subdirectory of the `$DIN_LOCK_ROOT` should be write-protected to exclude accidentally deleting or changing of them. 
As we are executing our CESM executable there is the utility `check_input_data` which is called to locate all the needed input data for a certain case. When this data is not found in `$DIN_LOCK_ROOT` it will automatically be downloaded by the scripts or the user using the `check_input_data` with -export as command argument.
If ones like to download the input manually it should be done __before__ building CESM. In addition it is also possible to download the data via svn subcommands direct, but it is much better to use the `check_input_data` script as it secures to download only the required data.

## CESM Creating And Configure A New Case

As we are executing our CESM executable there is the utility `check_input_data` which is called to locate all the needed input data for a certain case.
When this data is not found in `$DIN_LOCK_ROOT` it will automatically be downloaded by the scripts or the user using the `check_input_data` with -export as command argument.
If ones like to download the input manually it should be done __before__ building CESM. In addition it is also possible to download the data via SVN subcommands direct, but it is much better to use the `check_input_data` script as it secure to download only the required data.

### Prerequisites

We need the `perl-switch` for the project setup.

### Create a new case
Cases are pretty much a thing. But we don't know what they are...
To create a case execute `./scripts/create_newcase` with correct parameters, e.g.:

        # Parameters are respectively:
        # Case name and directory location
        # Machine name
        # Component set name
        # Resolution
        /create_newcase -case ./test1 \
            -mach userdefined \
            -compset B1850CN  \
            -res f45_g37

In the original repository many errors occur due to deprecated syntax and buggy setup code. We recommend to use our updated version of the code.
The text `Successfully created the case` should appear on your screen.

In case create_newcase breaks while calling one of the `mkbatch.*` scripts, you probably need to install CShell, as those scripts are written for `#!/bin/csh`.

### Setup case

Once a case has been created by the previous command, the setup for case has to be completed.
To achieve this, the `cesm_setup` script in the case directory needs to be executed.
Settings for this specific case are specified in `env_mach_pes.xml`. The documentation states that this file should only be manipulated using `xmlchange`.
As we want to use our own machine, we need to create a user defined machine for this test case.

Values that need to be set:

- `MAX_TASKS_PER_NODE` in `env_mach_pes.xml`
- `OS` in `env_build.xml`
- `MPILIB` in `env_build.xml`
- `COMPILER` in `env_build.xml`
- `EXEROOT` in `env_build.xml`
- `RUNDIR` in `env_run.xml`
- `DIN_LOC_ROOT` in `env_run.xml`

There is an example configuration in `scripts/example_config`. This configuration expects a folder in root `/cesm` and `/cesm/inputdata`, but if you don't have root access at your location, those variables can be easily changed (`EXEROOT`, `RUNDIR`, `DIN_LOC_ROOT`)

### Getting data
The data download script lies directly in `cesm1_2_1/scripts/"yourcase"` you created one step back.
To download input data to a specific data directory execute this, with an adjusted path.
        
         export DIN_LOC_ROOT='/Path/to/input/data/dir'
         mkdir -p $DIN_LOC_ROOT
        ./check_input_data -inputdata $DIN_LOC_ROOT -export -datalistdir $DIN_LOC_ROOT  

### Build the Case

	  cd ~/cesm/EXAMPLE_CASE
	  ./cesm_setup
	  ./EXAMPLE_CASE.build

# Conclusion
EDEX & CAVE are supported by the U.S. company Raytheon.

bla bla bla nice project.

# References

http://www.cesm.ucar.edu/models/current.html
http://www2.cesm.ucar.edu/
http://www.ecmwf.int/en/forecasts/
http://www.unidata.ucar.edu/software/awips2/
