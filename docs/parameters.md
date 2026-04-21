# Important Parameters

i3 works with a lot of shell variables that are used in the scripts. Some of them are important to understand and set correctly for your data. Here I will list some of the most important ones and what they do.


## general parameters
```bash
#
# General parameters
#
# prefix of directory names of alignment cycles
export DIRPRFX="cycle-"

# prefix of output file names
export CYCPRFX="gem-"

# directory with initial alignment parameters (relative to top directory)
export TRFDIR="trf_test"

# size of subvolumes
export MOTIFSIZE="64 64 64"

# sampling factor
export SAMPFACT="1 1 1"

# missing wedge compensation
export WDGCOMP="true"

```

These are pretty self-explanatory but here are some notes:
- `DIRPRFX` and `CYCPRFX` are just prefixes for the output directories and files. You can change them to whatever you want, but it is better to keep them consistent throughout the cycles.
- `TRFDIR` is the directory where the initial alignment parameters are stored. This is where the `.trf` files are created by the python code and read by the i3 scripts. 
- `MOTIFSIZE` is the size of the subvolumes that are cut out from the tomograms.
- `SAMPFACT` is the sampling factor for the subvolumes. I dont recommend changing this since it has weird effects on the alignment and classification steps.
- `WDGCOMP` is a boolean variable that indicates whether to use missing wedge compensation or not.


## Multireference Alignment Parameters (MRA)
```bash
#
# Parameters for raw motif alignment
#
#
# reference image or image stack (leave blank to use the global average
# or set to string "sel" to use selected class averages of previous cycle)
export REFIMG="../ref.mrc"

# mask applied to reference
# -mw for rectangular window, -md for ellipsoidal mask
# -ms is the mask apodization, -mc the mask center

export REFMSKOPT1=
export REFMSKOPT2=
export REFMSKOPT3=

# Fourier space filters applied to reference
# REFLOWPASS: diameter of low-pass filter mask
# REFLOWAPO:  low-pass apodization
export REFLOWPASS="0.35 0.35 0.35"
export REFLOWAPO="0.05 0.05 0.05"
# REFHIPASS:  diameter of high-pass filter mask
# REFHIAPO:   high-pass apodization
export REFHIPASS="0.060 0.060 0.060"
export REFHIAPO="0.007 0.007 0.007"

# additional filter stored in an image file (leave blank to skip)
# image size must be the corresponding transform size of MOTIFSIZE
export REFFILT=

# montages (true or false)
export REFMONT="true"
# number of columns in montage
#export REFMONTCOL="4"

# multireference alignment mask, applied to raw motifs
# -mw for rectangular window, -md for ellipsoidal mask
# -ms is the mask apodization, -mc the mask center
export MRAMSK=

# cross-correlation mode: xcf mcf pcf dbl (leave blank for xcf)
export MRACC="xcf"

# peak search radius
export MRAPKR="5 5 5"

# grid search
# for translational alignment only, use "0 0 0 0"
# for rotational alignment, use i"<nut> <nstep> <spin> <sstep>"
# search direction of rotation axis in concentric cones with
# maximum half-angle <nut> (nutation angle), and angular step <nstep>
# search rotation about the above axis from -<spin> to +<spin>
# (spin angle) with step size <sstep>
# rotational alignment about z-axis only: "0 0 <spin> <sstep>"
# angular ranges: 0 <= <spin> <= 180;  0 <= <nut> <= 180
export MRASRCH="0 0 0 0"
```

- The paths need to be with respect to the cycle-??? directory since that is where the scripts are executed. 
- Angular sampling is similar to Dynamo, but there is no cone-flipping: [Dynamo Multilevel Refinement](https://www.dynamo-em.org/w/index.php?title=Multilevel_refinement)
- For `MRACC` start with `xcf` and if you have a good reference and want to speed up the alignment you can try `mcf` or `pcf`. Normally we just stick to `xcf` since we can move to relion


## MSA and Classification Parameters
```bash
# size of extracted subvolumes for MSA
export MSAIMGSIZE="48 48 48"

# sampling factor for subvolume extraction
export MSAIMGSMP="${SAMPFACT}"


# file name of MSA mask, path name is relative to DIRPRFX,
# size must be defined above with MSAIMGSIZE or MSAPOLSIZE
# (leave blank to use mask creation options below)
export MSAMASK=

# MSA mask creation options
# -mw for rectangular window, -md for ellipsoidal mask
# -mc is the mask center
export MSAMASKOPT1="-md 40 40 40"  #"-md 24 24 24 -ms 3 3 3 -mc 0 0 8" #"-minv -md 18 18 18 -ms 3 3 3 -mc 0 0 8"
export MSAMASKOPT2= #"-mw 0 0 34 -mc 0 0 7"
export MSAMASKOPT3=
# file name of image to superimpose mask (leave blank to skip)
export MSAMASKSUPERPOS=

# create averages of raw stack
export MSAIMGAVG="false"

# image mask applied to extracted motifs
# -mw for rectangular window, -md for ellipsoidal mask
# -ms is the mask apodization, -mc the mask center
export MSAIMGMSK=

# Fourier space filters
# MSALOWPASS: diameter of low-pass filter mask
# MSALOWAPO:  low-pass apodization
export MSALOWPASS="0.36 0.36 0.36"
export MSALOWAPO="0.05 0.05 0.05"
# MSAHIPASS:  diameter of high-pass filter mask
# MSAHIAPO:   high-pass apodization
export MSAHIPASS="0.060 0.060 0.060"
export MSAHIAPO="0.007 0.007 0.007"
# additional filter stored in an image file (leave blank to skip)
# image size must be the corresponding transform size of MSAIMGSIZE
export MSAFILT=

# Maximal number of factors
export MSAFACT=40

# montage of eigenimages (true or false)
export MSAMONT="true"
# number of columns in montage
export MSAMONTCOL="8"
```

- Notice that the MSA particles dont have to be the same size and can be smaller to make things faster
- I will explain masking separately


## Classification Parameters
```bash
# Classification parameters
#
# number of classes to generate
# (single number or list of numbers separated by spaces)
export CLASSES="2 4 6"

# min/max number and increment of classes stored in the class-file
# the settings below store 2 4 6 8 classes
export CLSMIN="2"
export CLSMAX="10"
export CLSINC="2"

# factors used in classification
# (comma separated list, no spaces)
export CLSFACT="1-32"

# low-pass filter for montaged class averages (leave blank to skip montage)
export CLSMONT="0.35"
# number of columns in montage
#export CLSMONTCOL="10"

# fraction of ignored high-variance outliers
export CLSHVO="0.0"

# fraction of ignored high-variance class members
export CLSHVM="0.0"
```

- The number of classes is a very important parameter and you should try different numbers to see what works best for your data. 
- They need to be in the list of CLSMIN, CLSMAX, and CLSINC to be stored in the class-file and available for selection in the next step.
- The factors used in classification are also important and you should try different combinations to see what works
- I will explain how to redo classifications in the step-by-step guide, but you can change the factors and number of classes without redoing the alignment step, which is very useful for trying different settings and seeing what works best for your data.


## Alignment of Class Averages Parameters
```bash
# set specific reference image
# (leave blank for automatic evaluation of optimal reference)
export SELREFIMG=

# real space masks (maximum 3, applied sequentially)
# (leave blank for no mask)
export SELMSKOPT1="-md 80 80 80 -ms 5 5 5"
export SELMSKOPT2="-mw 0 0 40 -ms 5 5 5 -mc 0 0 7"
export SELMSKOPT3=
# Fourier space filters
# SELLOWPASS: diameter of low-pass filter mask
# SELLOWAPO:  low-pass apodization
export SELLOWPASS="0.35 0.35 0.35"
export SELLOWAPO="0.05 0.05 0.05"
# SELHIPASS:  diameter of high-pass filter mask
# SELHIAPO:   high-pass apodization
export SELHIPASS="0.060 0.060 0.060"
export SELHIAPO="0.007 0.007 0.007"
# additional filter stored in an image file (leave blank to skip)
# image size must be the corresponding transform size of MOTIFSIZE
export SELFILT=

# cross-correlation mode: xcf mcf pcf dbl (leave blank for xcf)
export SELCC="mcf"

# montages (true or false)
export SELMONT="true"
# number of columns in montage
#export SELMONTCOL="10"

# peak search radius
export SELPKR="4 4 4"

# radius for center of mass calculation
# export SELCMR="2 2 2"

# grid search (3D)
# for translational alignment only, use "0 0 0 0"
# for rotational alignment, use "<nut> <nstep> <spin> <sstep>"
# search direction of rotation axis in concentric cones with
# maximum half-angle <nut> (nutation angle), and angular step <nstep>
# search rotation about the above axis from -<spin> to +<spin>
# (spin angle) with step size <sstep>
# rotational alignment about z-axis only: "0 0 <spin> <sstep>"
# angular ranges: 0 <= <spin> <= 180;  0 <= <nut> <= 180
# grid search (21D)
# rotational alignment: "<rot> <step>"
# angular range: 0 <= <rot> <= 180
export SELSRCH="0 0 0 0"

# execution options
# use "nohup" to run on a single processor
# use "nohup <n>" to run on <n> processors
export SELOPT="nohup 50"

# selected classification to apply transformations to raw motifs
# must be one single number defined with CLASSES above, or leave blank to skip
export SELCLS="4"
```

- Unfortunately I have not been able to successully use `SELREFIMG`
- The masks and filters are similar to the MRA step, but they are applied to the class averages instead of the reference.
- The search parameters are also similar to the MRA step, but they are applied to the class averages instead of the raw motifs. 

[← Back: Getting Started](getting_started.md) | [Next: Step-by-step Example →](step-by-step.md)