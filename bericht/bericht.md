% I/O analysis of climate applications 
% Arne Beer, MN 6489196, Frank Röder, MN 6526113

\pagebreak

#Introduction
## About the paper

In this paper we analyse and present the pros and cons of different data structures required by a some carefully picked climate and weather prediction models. Further we look at the absolute bare minimum of data required by those models.

## Getting started
With intent to get an overview about the richness of climate,land,ice, weather and ocean models we took a look at some in depth to work out that the approachability and documentation was not that clear. Tons of very old models passed our way of searching through the sides of got dusty projects and source code. The question than was to get an good overview of up to date and easy to handle models which are still supported and updated.

## How we want to help
bla bla bla jeder kann sich an dem Paper schnell und effektive bedienen

# IFS - Integrated Forecasting System

## About IFS
IFS is a Model by European Centre for Medium-range Weather Forecast (ECMWF) which is used to make analysis of data. This data can be a variety of different physical bulks. This model looked quite promising as they offered an OpenIFS version of the model. After some research we discovered that the licence forbids "Commercial and benchmarking use of OpenIFS models", which stopped us from further investigation. I would recommend to use this model in a research or academic context, as there is plenty of documentation and a big user base.


# Unidata - Awips2

AWIPS2 is a package which contains weather forecast display and analysis. This open-source java application consists of EDEX a data server and CAVE the client for data analysis and rendering. 

## Installation
For the installation of awips2 ones can easily download the repository from github and make it run with installCave.sh and installEDEX.sh. Those install scripts use yum as a package manager are currently supported for CentOS, Fedora and RedHead. To make it compatible for the cluster there is maybe more to be done. Awips2 is normally installed with the help of the package manger YUM which could lead to some problems if you' re not the root. 
Awips2 requires a directory at root location "/awips2/". There are about 2000 lines of code where "/awips2/" is hardcoded, so switching directories is not an option.

\textbf{To build} a version for our purpose it would be the best to have a EDEX on the cluster which is providing our local CAVE with data for visualisation.

# CESM - Community Earth System Model

## About CESM
CESM itself consists of seven geophysical models like ocean, land, ice, atmosphere ... . The CESM project is made and supported by U.S. climate reseachers and mainly by the National Science Foundation (NSF). The scientific development is conducted by the CESM working group twice a year. For more information related to the development its recommended to visit the website [verlinkung zur quelle].

## Requirements
Here are some preconditions directly taken from the documentation of CESM.

* UNIX style operating system such as CNL, AIX and Linux
* csh, sh, and perl scripting languages
* subversion client version 1.4.2 or greater
* Fortran (2003 recommended, 90 required) and C compilers. pgi, intel, and xlf are recommended compilers.
* MPI (although CESM does not absolutely require it for running on one processor)
* NetCDF 4.2.0 or newer.
* ESMF 5.2.0 or newer (optional).
* pnetcdf 1.2.0 is required and 1.3.1 is recommended
* Trilinos may be required for certain configurations
* LAPACKm or a vendor supplied equivalent may also be required for some configurations.
* CMake 2.8.6 or newer is required for configurations that include CISM.

## Installation
- Open source
- Download at [CCMS(Link)](http://www.cesm.ucar.edu/models/cesm1.2/cesm/doc/usersguide/x290.html#download_ccsm_code):
    - Username: guestuser
    - Password: friendly
- Version 1.2.1
- Available with svn: 

        svn co https://svn-ccsm-models.cgd.ucar.edu/cesm1/release_tags/cesm1_2_1 \
        cesm1_2_1 --username guestuser --password friendly


Most parts of the CESM software project are open source. However three libraries are pulbished by the Los Almos National Laboratory, who licenced their software as free to use as long as it isn't used in a commercial context. Affected libraries are POP, SCRI and CICE [(Link to licence)](http://www.cesm.ucar.edu/management/UofCAcopyright.ccsm3.html).


# Conclusion
EDEX \& CAVE are supported by the U.S. company Raytheon.

bla bla bla nice project.

# References

http://www.cesm.ucar.edu/models/current.html
http://www2.cesm.ucar.edu/
http://www.ecmwf.int/en/forecasts/
http://www.unidata.ucar.edu/software/awips2/
