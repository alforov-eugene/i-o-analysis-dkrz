%I/O analysis of climate applications 
% Arne Beer, MN 6489196, Frank Röder, MN 6526113

\pagebreak

#Introduction
## About the paper and our goals

In this paper we analyse and present the pros and cons of different data structures required by a some carefully picked climate and weather prediction models.
Further we look at the absolute bare minimum of data required by those models. To consider the pre- and post-processing of data and the moment where it is really needed at a certain point is also a very
important question to mention. To get an idea about the importance of data management and processing we had to look at different kind of models for climate. 

In the following sections we will get into some models and show our progress while setting them up. At the end we will have a talk about the life-cycle of data.

## Getting started

With intent to get an overview about the richness of climate,land,ice, weather and ocean models we took a look at some in depth to work out that the approachability and documentation was not that clear.
Tons of very old models passed our way of searching through the sides of got dusty projects and source code. The question than was to get an good overview of up to date and easy to handle models which are still supported and updated. For this purpose we put a table together where we could choose from the best ones which were sounding promising to us.
We came across models with broken scripts and bad documentation, which was not very obvious on the first glance. So we take a path of trial and error or giving up, because
we faced broken scripts and problem deep inside the code.

\pagebreak

# IFS - Integrated Forecasting System

## About IFS
IFS has been chosen to be the first model for our research.
IFS is a Model by European Center for Medium-range Weather Forecast (ECMWF) which is used to make analysis of data. This data can be a variety of different physical bulks.
ECMWF offers a semi open-source version of their model for research institutions, which is called OpenIFS. The source code of this model can be obtained by requesting a license for the institution one is working for.
They provide a good documentation about their model, which cover instruction for building, running simulations as well as very detailed information about the mathematics and techniques used in their model.
After some research and two weeks passed we discovered a passage in their license, which forbids "Commercial and benchmarking use of OpenIFS models". As our original research goal is some kind of benchmarking we were forced to stop and switch to another model.
We still recommend to use this model in a research or academic context, as there is plenty of documentation and a big user base.


# Further progress
After the license incident with IFS we had to look for other open source models we could use for our research. We looked at many models in the following we will list some of the most promising:
WRF, CFS, GDAS, GFS, GEFS, CM2.X, GISS GCM ModelE, CESM, MITgcm, GEOSCI, Hector v1.0, MAGICC/SCENGEN, Metview, COSMO, SAGA, MPI-ESM, ECOHAM

The lookup for new models took about two weeks. Most of the models mentioned above had serious flaws which forbid us to use them in our research.
Many of them have stopped being maintained many years ago.
In some cases there was no license provided with no available support for clarifying legal questions. In other cases it was only possible to obtain a license for our specific research goal by buying it or it was even completely forbidden to use it for benchmarking purposes, as with OpenIFS.

Eventually we decided to focus our work on Community Earth System Model (CESM) and Ecosystem model, Hamburg (ECOHAM). Further we decided to take a look at AWIPS2 a tool for displaying processed weather forecast and a very good server for handling data.

\pagebreak

# Unidata - Awips2

AWIPS2 is a package which contains weather forecast display and analysis. This open-source `Java` application consists of `EDEX` a data server and CAVE the client for data analysis and rendering. 
Cite : "AWIPS II is a Java application consisting of a data display client (CAVE, which runs on Red Hat/CentOS Linux and OS X) and a backend data server (EDEX, which runs only on Linux)"
![AWIPS2 System](pics/awips2_coms.png "AWIPS2 Figure")

## EDEX (Environmental Data EXchange )
**EDEX** is the server for AWIPS2 which is used mainly for preparing the data for CAVE. There are different parts the server is containing of.

The first source for data is the LDM the Local Data Manager as a piece of software to share data with computers in other networks. The IDD (Internet Data Distribution) provides the LDM with data out of the Unidata community. The LDM can handle different kinds of data from National Weather Service data stream to
radar data, satellit images and grid data from numerical forecast models. The data could be get directly from the source or a LDM can communicate with another LDM.
When the LDM received data inside the EDEX, there is a message being send to the **Qipd** which is the Apache __Queue Processor Interface Daemon__ spreading the the availabilty of a data ready for processing.
The messages by the messaging system **Qipd** will contain also a file header for the EDEX to know which decoder to use.
EDEX can decode the data to make it ready for additional processing or telling CAVE that it is available for displaying. All of those messages are communicated via **edexBridge**
The default ingest server will handle the all data which are diffrent to grib messages. GRIB fully spelled __General Regularly-distributed Information in Binary form__ is a
data format by the ___WMO___ (World Meterological Organization) to encode results of weather models. The data is written binary into a table format. It's made effecient storage and transfer.
The PostgreSQL or Postgres in short is relevant for the storage and the requests of metadata, database tables and already decoded data. Postgres itself is a relational database 
management system which reads and store the EDEX metadata. The database size is not limited and it can handle 32 TB of database table capacity.

HDF5 fully spelled Hierarchical Data Format (v.5) is the main format used in AWIPS2 to store processed grids, images and so on. It is nowadays very similar to netCDF, which is
developed by Unidata. HDF5 can handle many different types just in one single file like data of multiple radars.

The __Python Process Isolated Enhanced Storage__ PyPIES created just for AWIPS2 is used for the writes and reads of data in HDF5 files. It is very similar to the Postgre but it
only processes the requests related to the HDF5 files. The intention was to isolate the EDEX from the HDF5 processes.


## CAVE (Common AWIPS Visualization Environment)
**CAVE** as a part of Awips2 is a tool for data visualisation and rendering. Its normaly installed on a seperated workstation apart from the other AWIPS2 parts.  
Cite: "CAVE contains of a number of different data display configurations called perspectives. Perspectives used in operational forecasting environments include D2D (Display Two-Dimensional), GFE (Graphical Forecast Editor), and NCP (National Centers Perspective).
![CAVE Example](pics/Unidata_AWIPS2_CAVE.png "CAVE Figure")

## Installation
For the installation of `Awips2` you can easily download the repository from Github and make it run with `installCave.sh` and `installEDEX.sh`. Those install scripts use yum as a package manager are currently supported for CentOS, Fedora and RedHead.
To make it compatible for the cluster there is maybe more to be done. Awips2 is normally installed with the help of the package manager YUM which could lead to some problems if you' re not the root. 
Awips2 requires a directory at root location "/awips2/". There are about 2000 lines of code where "/awips2/" is hard-coded, so switching directories is not an option.

**To build** a version for our purpose it would be the best to have a EDEX on the cluster which is providing our local CAVE with data for visualization.

\pagebreak

# CESM - Community Earth System Model

## About CESM
CESM itself consists of seven geophysical models like ocean, land, ice, atmosphere ... . The CESM project is made and supported by U.S. climate researchers and mainly by the National Science Foundation (NSF).
If there are different models in use, a so called coupler handle the time progression and overall management between coupled systems through sequences of communication.
The scientific development is conducted by the CESM working group twice a year. For more information related to the development its recommended to visit the website [link to source].
Additional CESM can be most of the time run out-of-the-box the developers claiming. "Bit-for-bit reproducibility" cannot be guarnteed, because of using different compilers and system versions.


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
* pnetcdf 1.2.0 is required and 1.3.1 is recommended (optional) 
* Trilinos may be required for certain configurations        X
* LAPACKm or a vendor supplied equivalent may also be required for some configurations.                \checkmark (Version 3.0)
* CMake 2.8.6 or newer is required for configurations that include CISM.        \checkmark (Version 2.8.12.2)

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
Will the problem with `create_newcase`remain, once should try one of the examples listed in the error message.

In case `create_newcase` breaks while calling one of the `mkbatch.*` scripts, you probably need to install CShell, as those scripts are written for `#!/bin/csh`.

The result of `create_newcase` is a directory `.../cesm/scripts/<YourCase>` with a bunch of directorys or filenames to be explained:

	README.case	- This files will contain tracked problems and changes at runtime
	CaseStatus	- A File containing a history of operations done in the actual case
	BuildConf/	- A never have to be edit directory with scripts for generating component namelists and utility libraries
	SourceMods/	- This directory is for modified source code
	LockedFiles/	- It contains copies of files that should not be changed, xml are locked until the clean operation is executed
	Tools/		- A never have to be edit location with support scripts
	env_mach_specific	- machine-specific variables for building/running are set here
	env_case.xml	- Case specific variables like the root and models are set(cannot be changed, have to re-run create_newcase for changes)
	env_build.xml	- contains the building settings, resolution and configuration options
	env_mach_pers.xml	- sets the machine processor layout
	env_run.xml	- contains run-time settings
	cesm_setup	- script for set up
	$CASE.$MACH.build	- script for building components, executables and utility libraries
	$CASE.$MACH.clean_build	- remove all object files and libraries
	$CASE.$MACH.l_archive	- script for long-term archiving of output (only if it is available on the machine)
	xmlchange	- utility to change values in other .xml files
	preview_namelists	- utility to see the component namelists
	check_input_data	- check for input datasets
	check_production_test	- creates a test of the owners case


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

### Build case

To build a case, the `$CASENAME.build` script needs to be executed.
In case you chose the `gnu` compiler in your settings, make sure you have `gmake` installed and create a symlink `gmake` to `make`.
If any previous builds failed, `$CASENAME.clean_build` has to be executed.

### Getting data
The data download script lies directly in `cesm1_2_1/scripts/"yourcase"` you created one step back.
To download input data to a specific data directory execute this, with an adjusted path.
        
         export DIN_LOC_ROOT='/Path/to/input/data/dir'
         mkdir -p $DIN_LOC_ROOT
        ./check_input_data -inputdata $DIN_LOC_ROOT -export -datalistdir $DIN_LOC_ROOT  

Now it also should be possible to check if the requiered data is present with follow command:

	check_input_data -inputdata $DIN_LOC_ROOT -check

To download missing data from the server use:

	check_input_data -inputdata $DIN_LOC_ROOT -export

Booth commands need to be run inside the `$CASEROOT`

### Build the Case

	  cd ~/cesm/EXAMPLE_CASE
	  ./cesm_setup
	  ./EXAMPLE_CASE.build

## Quickstart
This Quickstart should give an overview about the work flow of CESM especially when there is already a version ported to the local target machine. If there is nothing already ported on the start machine, start with the more detailed description above.

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
There is a list CESM supported components like [sets(Link)](http://www.cesm.ucar.edu/models/cesm1.2/cesm/doc/modelnl/compsets.html), [resolution(Link)](http://www.cesm.ucar.edu/models/cesm1.2/cesm/doc/modelnl/grid.html) and [machines(Link)](http://www.cesm.ucar.edu/models/cesm1.2/cesm/doc/modelnl/machines.html).
Remember, that the `-list` will always provide a list of supported component sets for the local CESM version.
The first letters of the `-compset` option will indicate which kind of model is used.
To create a case the command `create_newcase` is used. It creates a case directory containing the scripts and XML files to set up the configurations for resolution, component set and machine requested. The `create_newcase` has some arguments as condition and some additional options for generic machines. For more information `create_newcase -h` should help.
I case that a supported machine is in use `($MACH)` tipe the following words:

        >create_newcase -case $CASEROOT \
                -mach $MACH \
                -compset $COMPSET \
                -res $RES

When using the machine setting `userdefined` it is requiered to edit the resulting xml files and fill them with the informations needed for the target machine.
The `create_newcase -list` command will also show all available machines for the local version.
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

## Conclusion

We managed to fix the build scripts to create and setup a case. In the step of compiling the code we encountered a few errors, but we were able to compile two models. After that another error occurred during compilation of the `pio` module.
CESM ships with a parallel IO library, which practically is a set of interfaces for netcdf, parallel netcdf or binary IO. We chose to use pnetcdf for our build.
After installing the required libraries and setting the proper paths and variables as described in their documentation the build still failed and required a configuration file for `pio`. There is no further information about this configuration file in their documentation.
After writing their support without response, we were forced to stop using CESM.

After all CESM still looks like a promising model, but it has many flaws. Their build scripts aren't generic. Nearly 3 weeks were spent to understand the code and fix old syntax or hard-coded paths.
If there were better documentation about the setup and compilation for CESM we could've probably used it, as we were really close to compiling it.


# ECOHAM5

## About
ECOHAM5

__research about ECOHAM5 not ECHAM__

... is a model for atmospheric circulation, which was developed by the Max Planck Institute for Meteorology. It's a important part of the MPI-ESM which is the 
Earth system model of the Max Planck Institute. ECHAM itself is branched in year 1987 from the global numerical weather prediction model by ECMWF. Ever science its developed by Max Planck Institute.

## Getting Started

### Source Code
Not freely available in the internet.

### Compile ECOHAM5
To compile ECOHAM5 for a testcase, there is the following script to be run in different ways:

	./CompileJob-cluster.sh TEST 0  // for just the compiling
	./CompileJob-cluster.sh TEST 1	// for compiling and make it ready to run
	./CompileJob-cluster.sh TEST 2	// compiling and run the model

In our case `TEST` is the data input. If an error occures and you don't have the permission to run `sbatch` on a specified partiotions, it is nessecarry to make modification in the `RunJob.TEST` located in the folder of the declared input. The wrk directory will contain the models ouput. 
The directory `Input` inside the wrk folder contain all the input used for the model run. There are `.dat` `.header`and `.direct` files which are providing the application
with all data needed.

# Lifecycle of data

## General
Through the whole process of running a simulation there a different type of data at certain points. The complexity and the information can differ.
The data which is fed into the program at the beginning won't be the same which is visualized by a color on the climate overview

The Lifecycle could be divided into those parts:

1. creating data
2. processing data (pre- and post-processing included)
3. analyzing data
4. preserving data
5. giving access to data
6. re-using data

### Creating the data

Creating the data could also be named as design of the research because it will limit and lead the handling of the application. Which kind of data management, formats and storage used is another question to be answered. If there are already similar simulations, data can be found there for re-use. For not already exsisting simulation the data must be collected through experiments, measures and also simulations generation data. Metadata will also be captured and maybe created.

### Processing data
This step will contain the digitization, transcribing and translation of data into a useful figure. It also is about the vetting of validate and clean data. To anonymize data needed could also be part of processing. Sometimes there is pre- and post-proscessing apart from the general processing. Pre-processing will prepare the particullar data
just before a certain step. It will dimiss all the not needed information to save time and storage capacity. After such a step there could be another process called post-processing
which is about the treatment of data to make it useable for other steps afterwards.
To make it easy to read for other people, the data should be described and explained. As the last step it is nescessary to take a look at the storing of data.


### Analyzing of data

Because data itself is not very enlightening, especially when there are thousands of values we need methods to interpret them. Derivation of data while calculating the output of the research and making publications is another part. Now that the made our main calculations with the data we have to prepare it for preservation.

### Preserving of data

Preserving of data is about checking out the best formats and best mediums on which we backup and store the data. Also creation of metadata and documentation is important for
the final archiving.

### Giving access to data

Because reseach is done mostly by public institutes there is a demand for sharing and distribute the data and knowledge. To consider about the access control and copyrights
might also be very useful.

### Re-using data

Researches in the future could also be based on the work which was done in the past. Teaching and publications may be good examples for re-using.

## Summary

With the growth of data in simulations and the improvement of processing power which is much higher than the performace of store power it is very essential for research establishments. The data and knowledge are the key and the only thing those institutes are working for. Therefore data and knowledge needs the right treatment.


# Conclusion

This project was about the analysis of input and output of climate applications. We made our way through run models providided by our supervisors and some we found in
the internet which seemed suiteable for our purpose. We faced bad documentations and insufficient scripts for making such a model run. At the end we did not get along
with most of them at different levels progress. ECHAM was the only one which worked on the fly, because of the test which is very well prepared. Apart of that it didn't keept us
away from analysing the life-cycle of data in general and make a little summary about that.

EDEX & CAVE are supported by the U.S. company Raytheon.

# References
https://www.unidata.ucar.edu/software/ldm/ldm-current/factsheet.html

http://www.cesm.ucar.edu/models/current.html
http://www2.cesm.ucar.edu/
http://www.ecmwf.int/en/forecasts/
http://www.unidata.ucar.edu/software/awips2/
https://unidata.github.io/awips2/
https://www.bu.edu/datamanagement/background/data-life-cycle/  Data Lifecycle
