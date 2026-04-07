# Getting Started

This is how ytou get started after picking your particles manually in dipole picking method. You will need .pos files that has a 3 columns for x, y, z coordinates and the rows are in order of first and second pick of each particle.

Then in `janlevinson` you need to make a directory and put all the pos files in it. 

## Running the python code
- First run 
```bash
conda activate cc3d
```
- Then run 
```bash
python ~/bin/pos2i3.py --pos pos/ --scale 1 --random
```
- `--pos` is the directory with pos files that are assumed to be named the same as tomograms, only with .pos instead of .mrc
- You might need to rename these files, you can use the `rename 's/FIND/REPLACE/g' *.pos` for bulk renaming
- ***warning*** this might be difficult to undo, make sure you have a copy somewhere.
- `--scale` is to change the binning. this way you can pick on bin 8 and run on bin2 tomograms.
- ***warning*** I have seem inconsistencies in scaling. Also the contrast will go down significantly when you scale up
- `--random` if you want to randomize the angle around z to get out of missing wedge. you may want to start with this, or do a round of alignment and then do the randomization


## Created files and directories
1. ### The definitions directory: `defs/`
In this directory there will be two files
`defs/maps`: this file has 3 columns, the directory of the tomograms, the name of the tomogram, and the path to the tlt file for that tomogram. The tlt file has the range of tilts for missing-wedge compensation later. If assumes -60 to 60 with 2 degree step, but you can change it if your data is different. You will need to edit this file in `~/bin/` but I dont think this makes any significant difference.  
```bash
../maps/ NAD054_1_lam12_ts_007.mrc_13.30Apx.mrc ../tlt/NAD054_1_lam12_ts_007.mrc_13.30Apx.tlt
../maps/ NAD054_1_lam12_ts_005.mrc_13.30Apx.mrc ../tlt/NAD054_1_lam12_ts_005.mrc_13.30Apx.tlt
../maps/ NAD055_3_lam15_ts_008.mrc_13.30Apx.mrc ../tlt/NAD055_3_lam15_ts_008.mrc_13.30Apx.tlt
../maps/ NAD054_1_lam7_ts_002.mrc_13.30Apx.mrc ../tlt/NAD054_1_lam7_ts_002.mrc_13.30Apx.tlt
../maps/ NAD054_1_lam12_ts_004.mrc_13.30Apx.mrc ../tlt/NAD054_1_lam12_ts_004.mrc_13.30Apx.tlt
../maps/ NAD054_1_lam15_ts_002.mrc_13.30Apx.mrc ../tlt/NAD054_1_lam15_ts_002.mrc_13.30Apx.tlt
```

`defs/sets`: This file has 2 columns, the name of the tomogram and name of the `.trf` file that has the particles from that tomogram. The `.trf` file is created by the python code and I will explain it later.
```bash
NAD054_1_lam12_ts_007.mrc_13.30Apx.mrc NAD054_1_lam12_ts_007.mrc_13.30Apx
NAD054_1_lam12_ts_005.mrc_13.30Apx.mrc NAD054_1_lam12_ts_005.mrc_13.30Apx
NAD055_3_lam15_ts_008.mrc_13.30Apx.mrc NAD055_3_lam15_ts_008.mrc_13.30Apx
NAD054_1_lam7_ts_002.mrc_13.30Apx.mrc NAD054_1_lam7_ts_002.mrc_13.30Apx
NAD054_1_lam12_ts_004.mrc_13.30Apx.mrc NAD054_1_lam12_ts_004.mrc_13.30Apx
```


2. ### The maps directory: `maps/`
This is where you put the tomograms. The names of the tomograms should match the names in the `defs/maps` file. You can also put the tomograms somewhere else and soft-link here. At any point you can change between raw, denoised, sirt, etc. tomograms, just make sure the names match.

This will not be done by the code and needs to be donr **manually** by the user.


3. ### The trf directory: `trf/`
This is where the main function of the code happens. The python code will read the pos files and create a `.trf` file for each tomogram. 
The `.trf` file has XX columns:
    - **Column1:** Name of the particle, which is tomogram name + particle number (e.g., NAD054_1_lam12_ts_007.mrc_13.30Apx_0001)
    - **Column2-4:** x, y, z coordinates of the particle
    - **Column5-7:** \Delta x, \Delta y, \Delta z for the particle, which are 0.0 to begin with
    - **Column8-17:** 9 values for the rotation matrix, i.e. 1 0 0 0 1 0 0 0 1 is not rotation, etc.
```
NAD054_1_lam12_ts_004.mrc_0000000 46 421 72 0.0 0.0 0.0 0.25045375749633164 -0.9450187992901435 0.2102674115125402 0.09737071909722955 -0.19149986131638969 -0.976650780053081 0.9632195276355524 0.2650797466877067 0.04405530022056553
NAD054_1_lam12_ts_004.mrc_0000001 449 474 69 0.0 0.0 0.0 0.42774992428180214 0.8057929694911962 -0.40954571490307706 -0.34729320510027895 -0.2717994224956718 -0.8975034839053242 -0.8345162862209987 0.5261394912469308 0.16358424062950772
NAD054_1_lam12_ts_004.mrc_0000002 495 588 80 0.0 0.0 0.0 0.4589448088207747 -0.877093460401622 -0.14169235751151515 -0.7815058323258316 -0.3226645287341817 -0.5339814940027957 0.42263257860365183 0.35580143847019063 -0.8335388652518677
```

4. ### The tlt directory: `tlt/`
This is where you put the tilt files. The names of the tilt files should match the names in the `defs/maps` file. The tlt files are in the old protomo formatn and I dont recommend changing them. This is just used to create a wedge-mask in the fourier space for alignment and classification steps to compensate for the missing wedge.
```bash
TILT SERIES TOMOGRAM
AXIS
TILT AZIMUTH 90.0
IMAGE 0 FILE TOMOGRAM_3 ORIGIN [463 479] TILT ANGLE -51.00 ROTATION 0.000
IMAGE 1 FILE TOMOGRAM_4 ORIGIN [463 479] TILT ANGLE -48.00 ROTATION 0.000
IMAGE 2 FILE TOMOGRAM_5 ORIGIN [463 479] TILT ANGLE -45.00 ROTATION 0.000
...
```
You can try different trf directories with different particles and settings by changing the 

```bash
# directory with initial alignment parameters (relative to top directory)
export TRFDIR="trf_test"
```

---

[← Back: Overall Description](overall-description.md) | [Next: Parameters Directory →](parameters.md)