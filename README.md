# FSL_FIX Setup for MacOS

The FIX package requires FSL, R and one of MATLAB Runtime Component, full MATLAB or Octave. We recommend the use of the MATLAB Runtime Component.

## PreRequires: 
1. FSL
2. XCode (from App Store) 
3. command line tools
~~~bash
xcode-select --install
~~~
or Xcode->Open Developer Tool->More Developer Tools

## Install R
~~~bash
. $FSLDIR/fslpython/bin/activate $FSLDIR/fslpython/envs/fslpython
~~~
If your FSLDIR folder requires admin rights to modify (e.g. it's in /usr/local) then
prepend these commands with sudo.
~~~bash
$FSLDIR/fslpython/bin/conda install R=3.4
$FSLDIR/fslpython/bin/conda install r-rocr=1.0_7 r-kernlab=0.9_25 r-randomForest=4.6_12 r-class=7.3_14 r-e1071=1.6_8 r-devtools
~~~
Launch R with:
~~~bash
$FSLDIR/fslpython/envs/fslpython/bin/R
~~~
and run the following (with sudo if FSLDIR modifications needs admin rights):
~~~R
require(devtools)
chooseCRANmirror()
install_version("mvtnorm", version="1.0-8")
install_version("multcomp", version="1.4-8")
install_version("coin", version="1.2-2")
install_version("party", version="1.0-25")
~~~
Inside settings.sh set FSL_FIX_R_CMD to $FSLDIR/fslpython/envs/fslpython/bin/R

## Install MATLAB Compiler Runtime (MCR)

Requires: MATLAB Style environment

If there is a folder 'compiled' in the FIX folder then the MATLAB portions have
been compiled for running without MATLAB. To use this you need to install the
MATLAB Compiler Runtime (MCR) version that FIX was compiled against. To identify this
run the script:
~~~bash
./mcr_script.sh
~~~
This will give the URL to download the MCR from and indicate which version you need.
Once you have downloaded the MCR you need to install it somewhere and then set
FSL_FIX_MCRROOT in 'settings.sh' to refer to the folder containing the runtime(s), e.g.
if you have a folder /usr/local/mcr which contains the runtime v93, then FSL_FIX_MCRROOT
should be set to /usr/local/mcr.

## Configure

Ensure the FSL_FIX_CIFTIRW and FSL_FIX_WBC variables in settings.sh are pointed
at your HCP Workbench MATLAB CIFTIRW and Workbench folders respectively.

## MATLAB Compilation

~~~bash
cd fix folder
./build_MATLAB
~~~
If the script cannot automatically find your 'matlab' command, configure settings.sh
to point at the matlab and mcc programs.

This will install the binaries into 'compiled/`OS`/`arch`' (eg
compiled/Darwin/x86_64) and create a file 'MCR.version'
containing the version number of the MATLAB Compiler Runtime necessary to run
this program.

If you wish to build a re-distributable tar.gz file of the compiled software you
should run ./build_MATLAB on each platform of interest (e.g. Linux and macOS),
collating the compiled/Linux and compiled/Darwin folders into a single copy of the
fix folder and then run:
~~~bash
./build_MATLAB release
~~~
This will generate fix.tar.gz in the fix folder which can then be distributed.
