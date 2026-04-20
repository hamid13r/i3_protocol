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
The files created in this step are:
| File | Description | Preview |
|----------|----------|----------|
| filament-000.i3d   | This is the i3 file that has the aligment info, etc.   |  N/A    |
| filament-000-raw-totsum.img   | Global average from the initial coordinates and rotations   |   ![global average](/media/raw-totsum.png)    |
| filament-000-raw-totsum-y.mrc   | Global average rotated for side view   |    ![global average y](/media/raw-totsum-y.png)     |
| param.sh  | Copy of param-template.sh for editing in the cycle if needed  |    ![global average z](/media/raw-totsum-z.png)     |
[Detailed instructions for setting up the protocol]

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
| Data 1   | Data 2   |      |
| Data 4   | Data 5   |       |

## Step 3: Mutltireference Alignment
The settings are controlled by the `cycle-000/param.sh` in the `MRA` section. You need to run:

```bash
i3jalign.sh 0
```
This will run the alignment for this cycle. The files created in this step are:
| File | Description | Preview |
|----------|----------|----------|
| Data 1   | Data 2   |      |
| Data 4   | Data 5   |       |



## Step 4: MSA
After setting the variables in the `MSA` section of the `param.sh` file, you need to run:
```bash
i3jmsa.sh 0
```

This creates the factors and the files created in this step are:
| File | Description | Preview |
|----------|----------|----------|
| Data 1   | Data 2   |      |
| Data 4   | Data 5   |       |

## Step 5: Classification
After setting the variables in the `CLS` section of the `param.sh` file, you need to run:
```bash
i3jclass.sh 0
```

This creates the classes and the files created in this step are:
| File | Description | Preview |    
|----------|----------|----------|
| Data 1   | Data 2   |      |
| Data 4   | Data 5   |       |

## Step 6: Aligning the Selected Class Averages
After setting the variables in the `SEL` section of the `param.sh` file, specially the `SELCLS` that controls which classification to use, you need to run:
```bash
i3cp.sh 0
i3jselect.sh 0
```

This will aling the class averages and creates the files:
| File | Description | Preview |    
|----------|----------|----------|
| Data 1   | Data 2   |      |
| Data 4   | Data 5   |       |

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
