# i3 Protocol Description

## Overview

If you are reading this protocol, you are one of lucky people who work on the subvolume averaging of a target that is specifically located and relatively rare in your tomograms. However, this might be one the few ways of actually getting somewhere so it is not all bad! 

i3 is a software package developped in labs on Ken Taylor and later in [The Liu Lab](https://medicine.yale.edu/lab/jun-liu/).

We have been using it on our targets including Prohibitin and ATPSynthase so I am writing my protocol to help with others who might want to use it as well

## Some general ideas

### The cases for using i3:
1.	You have a rare particle with limited number (200-2500 usually)
2.	Your particles seem to need help to align initially before more can be done in relion/Dynamo
3.	Membrane-bound particles that you picked manually using dipole picking
4.	Some classifications to see heterogeneity that couldn’t be squeezed out in relion (has to be low resolution heterogeneity: large parts, attached protein, etc)


### Why does it work where others softwares fail?
The entire method of i3 is different than other packages since it was developed for plastic embedded, stained tomograms. The method is called **"alignment through classification"**: when you have a population of particles there are 2 sources of heterogeneity: first one is real particles being different, second is particles are just misaligned. Because the raw subvolumes are noisy, one can benefit from finding those that are misaligned the same way and then align the class averages to each other. Doing this iteratively will help you get a reasonable alignment of all particles and you can move on to the actual heterogeneity if possible or needed.
Also as you will see the process is very much user-guided and you will need to be careful not to introduce user bias and get fake results from noise. 

### Downsides of i3
It is often very annoying! There are unwritten rules and tricks that I am hoping to help with in this protocol. 
It also can be very slow based on number of particles and changing the binning takes time since you need the whole tomogram. 
i3 is executed almost entirely through shell-scripts, so some of the shell problem will make their way through when you are working with it.
 

## Requirements

- Linux
- The binaries to copy and use
- CPUs are using for parallelization of the alignment step

---

[Next: Step-by-Step Guide →](step-by-step.md)
