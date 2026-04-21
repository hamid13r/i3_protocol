# Step-by-Step Guide

## Prerequisites

As we discussed you need your tomograms and all the other files in place to get started. I will show you one cycle of the workflow with an example here, these are some tomograms that have amyloid fibrils in them, and I will show you how to get the global average, create the reference, do the multireference alignment, MSA, classification, and then align the class averages. The pos files were created using dragonfly and amira and then I wrote a python code to convert them to the format needed for i3.

## Step 1: Creating the Global average
First you need to run:
```bash
loadi3 
```
which is an alias to `source /opt/i3/0.9.9.3/setup.sh; export PATH="~/i3bin/:~/bin/:$PATH"; source /home/janlevinson/.zshrc;` and then run the following command to create the global average:
```bash
i3jinitial.sh
```
or 
```bash
i3jnext.sh {cycle number} {cycle number + 1}
```
The files created in this step are:
| File | Description | Preview |
|----------|----------|----------|
| filament-000.i3d   | This is the i3 file that has the aligment info, etc.   |  N/A    |
| filament-000-raw-totsum.img   | Global average from the initial coordinates and rotations   |   ![global average](/media/raw-totsum.png)    |
| filament-000-raw-totsum-y.mrc   | Global average rotated for side view   |    ![global average y](/media/raw-totsum-y.png)     |
| param.sh  | Copy of param-template.sh for editing in the cycle if needed  |   N/A   |

If you want to undo this part you need to delete the `cycle-000/` or the last cycle you created. 

```
# Example command or code snippet
example command here
```

## Step 2: Creating the Reference
Here you need to edit the `cycle-000/param.sh` file and change the `REFIMG` variable: leave empty for global average or point to a file. Then run:
```bash
i3jreference.sh 0
```
This will create the reference for this cycle, I believe masked and filtered. 
Files created in this step are:
| File | Description | Preview |
|----------|----------|----------|
| filament-000-ref.img   | stack of the reference images (can be viewed if there is 1)  |  N/A    |
| filament-000-ref.fou   | Fourier transform of the reference stack   |    N/A   |
| filament-000-ref-mont.img   | Montage of the reference images, filtered and can be viewed  |     ![reference montage](/media/refmont.png)   |
| filament-000-ref-y-mont.img   | Montage of the reference images rotated for side view   |   ![reference montage y](/media/refmont-y.png)     |

If you are not happy with the results, edit the `param.sh` variables and run the command again, it will overwrite the previous files. You can also change the reference image to something else if you want to try a different approach.


## Step 3: Mutltireference Alignment
The settings are controlled by the `cycle-000/param.sh` in the `MRA` section. You need to run. Particles in each trf file will be processed by a CPU in parallel, so the number of CPUs alone will not 

```bash
i3jalign.sh 0
```
This will run the alignment for this cycle. The files created in this step are:
| File | Description | Preview |
|----------|----------|----------|
| filament-000-old.i3d   | Data 2   |  N/A    |
| filament-000.i3d  | Data 5   |    N/A   |
| i3alignrun.sh | des |  N/A   |
| i3align/ | directory with the aligned particles and their info  |   N/A  |


If you need to redo this part you can delete the `i3align/` directory and then `i3jalign.sh 0`, change the prameters if needed and run the command again.


## Step 4: MSA
After setting the variables in the `MSA` section of the `param.sh` file, you need to run:
```bash
i3jmsa.sh 0
```

This creates the factors and the files created in this step are:
| File | Description | Preview |
|----------|----------|----------|
| filament-000-svd.log   | Log file showing the calculation and the progress  |      |
| filament-000-msamask.img  | Mask image for MSA created based on parameters |     ![msa-mask](/media/msa-mask.png)  |
| filament-000-svd.dat  | dat file showing the contribution of each factor on column 2  |     N/A    |
| filament-000-mont.rsv | Montage of the factors created by MSA, can be viewed  |     ![msa-factors](/media/msa-factors.png)    |
| filament-000-mont-y.rsv | Montage of the factors created by MSA rotated for side view, can be viewed  |     ![msa-factors y](/media/msa-factors-y.png)    |

If you need to redo this part edit the parameters in the `param.sh` file and run the command again, it will overwrite the previous files. 
## Step 5: Classification
After setting the variables in the `CLS` section of the `param.sh` file, you need to run:
```bash
i3jclass.sh 0
```

This creates the classes and the files created in this step are:
| File | Description | Preview |    
|----------|----------|----------|
| filament-000-class.i3d  | Data file with the classification info  |  N/A    |
| filament-000-class.cls   | File withinfo about classification   | N/A     |
| filament-000-class.log  | Log file showing the classification process and progress  | N/A     |
| filament-000-class-004/ | Directory with the particles with 4 classes and their info  | N/A     |
| filament-000-class-004/dendr-004.ps | | Dendrogram showing the relationship between the classes  | ![dendrogram](/media/dendrogram.png)     |
| filament-000-class-004/004.counts  |  File showing the number of particles in each class  | N/A   |
| filament-000-class-004-mont.img | Montage of the class averages, can be viewed  | ![class mont](/media/class-mont.png)     |
| filament-000-class-004-y-mont.img | Montage of the class averages rotated for side view, can be viewed  | ![class mont y](/media/class-mont-y.png)     |

Same files will be created for the number of classes you set in the `param.sh` file. The `avg` files in th class directory are unfiltered.

If you need to redo this part edit the parameters in the `param.sh` file and then `rm -r cycle-000/*class*` (BE CAREFUL NOT TO DELETE OTHER FILES) and then run the command again.

## Step 6: Aligning the Selected Class Averages
After setting the variables in the `SEL` section of the `param.sh` file, specially the `SELCLS` that controls which classification to use, you need to run:
```bash
i3jcp.sh 0
i3jselect.sh 0
```
The first command makes copied of the selected class averages and the second command runs the alignment of the class averages.

This will aling the class averages and creates the files:
| File | Description | Preview |    
|----------|----------|----------|
| filament-000-ali.i3d  | Data file with the alignment info of class averages  |   N/A   |
| filament-000-sel-mont.img   | Montage of the selected class averages, can be viewed  |   ![sel mont](/media/sel-mont.png)    |
| filament-000-sel-y-mont.img   | Montage of the selected class averages rotated for side view, can be viewed  |   ![sel mont y](/media/sel-mont-y.png)    |
| filament-000-sel-mask.img | Mask image for the selected class averages |   ![sel mask](/media/sel-mask.png)    |

You can see that they are rotated to align to each other, this is less prone to bias compared to aligning raw subvolumes since the class averages have higher SNR. There is a way to align them to a single reference but I have not found a way to make it work yet.

If you need to redo this part, you need to first run `rm -r cycle-000/*sel* cycle-000/filament-000-ali/` (BE CAREFUL NOT TO DELETE OTHER FILES) and then run the commands again, edit the parameters if needed.

## Step 7: Next Cycle
At the end of a cycle you need to run:
```bash
i3jnext.sh 0 1
```

This will be the same as `i3jinitial.sh` but for the next cycle. The files created in this step are:
| File | Description | Preview |    
|----------|----------|----------|
| Data 1   | Data 2   |      |  
| Data 4   | Data 5   |       |
# Example verification
verification example here
```

## Troubleshooting

### Issue 1
**Problem**: [Description]
**Solution**: [How to fix it]

### Issue 2
**Problem**: [Description]
**Solution**: [How to fix it]

---

[← Back: Description](description.md) | [Next: Example Workflow →](example-workflow.md)
