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
IFS is a Model by European Centre for Medium-range Weather Forecast (ECMWF) which is used to make analysis of data. This data can be a variety of different physical bulks.
This model looked quite promising as they offered an OpenIFS version of the model.
After some research we discovered that the licence forbids "Commercial and benchmarking use of OpenIFS models", which stopped us from further investigation.
I would recommend to use this model in a research or academic context, as there is plenty of documentation and a big user base.


# Unidata - Awips2

AWIPS2 is a package which contains weather forecast display and analysis. This open-source java application consists of EDEX a data server and CAVE the client for data analysis and rendering. 

## Installation
For the installation of awips2 ones can easily download the repository from github and make it run with installCave.sh and installEDEX.sh. Those install scripts use yum as a package manager are currently supported for CentOS, Fedora and RedHead.
To make it compatible for the cluster there is maybe more to be done. Awips2 is normally installed with the help of the package manger YUM which could lead to some problems if you' re not the root. 
Awips2 requires a directory at root location "/awips2/". There are about 2000 lines of code where "/awips2/" is hardcoded, so switching directories is not an option.

**To build** a version for our purpose it would be the best to have a EDEX on the cluster which is providing our local CAVE with data for visualisation.

# CESM - Community Earth System Model

## About CESM
CESM itself consists of seven geophysical models like ocean, land, ice, atmosphere ... . The CESM project is made and supported by U.S. climate reseachers and mainly by the National Science Foundation (NSF).
The scientific development is conducted by the CESM working group twice a year. For more information related to the development its recommended to visit the website [verlinkung zur quelle].

## Requirements
Here are some preconditions directly taken from the documentation of CESM.
In favour to make it run on the cluster we are working with we have to walk through the list:

* UNIX style operating system such as CNL, AIX and Linux	\checkmark	
* csh, sh, and perl scripting languages		\checkmark	
* subversion client version 1.4.2 or greater	\checkmark	
* Fortran (2003 recommended, 90 required) and C compilers. pgi, intel, and xlf are recommended compilers.	\checkmark (gfortran gcc-Version 4.8)
* MPI (although CESM does not absolutely require it for running on one processor)	\checkmark
* NetCDF 4.2.0 or newer.	\checkmark (Version 7.3 & 4.2)
* ESMF 5.2.0 or newer (optional).
* pnetcdf 1.2.0 is required and 1.3.1 is recommended	??? 
* Trilinos may be required for certain configurations	X
* LAPACKm or a vendor supplied equivalent may also be required for some configurations.		\checkmark (Version 3.0)
* CMake 2.8.6 or newer is required for configurations that include CISM.	\checkmark (Version 2.8.12.2)

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

Most parts of the CESM software project are open source. However three libraries are pulbished by the Los Almos National Laboratory, who licenced their software as free to use as long as it isn't used in a commercial context. Affected libraries are POP, SCRI and CICE [(Link to licence)](http://www.cesm.ucar.edu/management/UofCAcopyright.ccsm3.html).


## Input Data Set
### Setup
There is actually a set of input data which can be downloaded and configured for CESM. It can be made available through another Subversion input data repository by using the same username as used in the installation above.
The dataset is around 1 TByte big and should not be downloaded at ones. The download is regulated on demand, so if CESM needs the particullar data it will be downloaded and checked automatically be CESM itself. The data should be on a disk in the local area.
The CESM variable `$DIN_LOCK_ROOT` has to be set inside of the script. Multiple users can use the same `$DIN_LOCK_ROOT` directory and should be configurated as group writeable.
If the machine is supported there is a preset otherwise there is a possibility to make it also run on generic machines with the varibale argument `create_newcase`.
Files in the subdirectory of the `$DIN_LOCK_ROOT` should be write-protected to exclude accidentally deleting or changeing of them. 
As we are executing our CESM executable there is the utility `check_input_data` which is called to locate all the needed input data for a certain case.
When this data is not found in `$DIN_LOCK_ROOT` it will automatically be downloaded by the scripts or the user using the `check_input_data` with -export as command argument.
If ones like to download the input manually it should be done __before__ building CESM. In addition it is also possible to download the data via svn subcomands direct, but it is much better to use the `check_input_data` script as it secure to download only the required data.
	
### Getting data
The data download script lies in `cesm1_2_1/scripts/ccsm_utils/Tools`.
To download input data to a specific data directory execute this, with an adjusted path.
	
         export DIN_LOC_ROOT='/Path/to/input/data/dir'
         mkdir -p $DIN_LOC_ROOT
        ./check_input_data -inputdata $DIN_LOC_ROOT -export -datalistdir $DIN_LOC_ROOT       


# Conclusion
EDEX & CAVE are supported by the U.S. company Raytheon.

bla bla bla nice project.

# References

http://www.cesm.ucar.edu/models/current.html
http://www2.cesm.ucar.edu/
http://www.ecmwf.int/en/forecasts/
http://www.unidata.ucar.edu/software/awips2/
