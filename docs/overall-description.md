# i3 Protocol Cycle Steps

Every cycle of i3 goes through the following steps:

1. **Subvolume Extraction and Averaging** (`i3jinitial.sh` or `i3jnext.sh`)
    - Subvolumes are cut out and averaged using the initial alignment or the alignment from the last cycle

2. **Reference Selection** (`i3jreference.sh $num`)
    - The reference can be provided, or can be the global average, or the aligned class-averages of the last cycle

3. **Multi-Reference Alignment (MRA)** (`i3jalign.sh $num`)
    - Subvolumes are aligned to the reference
    - Configurable parameters:
      - Binary alignment mask (soft mask required) for region-specific alignment
      - Filtering: Low-pass and high-pass Fourier frequencies in x, y, z (0.5 = full resolution, 0.25 = 2× pixel size)
      - Search ranges: `- \Delta x` to `+ \Delta x` in x, y, z (cannot be zero; use 0.1 to skip)
      - Angular search: `cone-range cone-sampling rotation-range rotation-sampling`
    - Note: Sometimes skipped due to reference bias susceptibility (0.1 for ranges and 0 for all angles)

4. **Multivariate Statistical Analysis (MSA)** (`i3jmsa.sh $num`)
    - Uses principal component analysis to identify dataset heterogeneities
    - Optional mask for region-specific analysis
    - Eigen-vectors are saved as `cycle-???/${CYCPRFX}-${num}-y-mont.rsv`
    - when seeing those factors notw **Factor 0** is often the global average and **Factors 1–2** Typically represent missing-wedge artifacts
    - **Remaining factors**: Detected heterogeneities, they may have inverted contrast and reflect the data but they are often difficult to interpret

5. **Classification** (`i3jclass.sh $num`)
    - Projects dataset onto selected factors
    - Supports multiple class ranges (e.g., 2–20 with step of 2 produces: 2, 4, 8, …, 20)
    - Subset selection possible (e.g., 2, 4, 10)
    - There is also a dendogram showing the relationships between the classes

6. **Copy Selected Classification** (`i3jcp.sh $num`)
    - Choose optimal classification balancing heterogeneity vs. particle count

7. **Class-Average Alignment** (`i3jselect.sh $num`)
    - Align class-averages to each other
    - All the same controls as the MRA section are available
    - the resulting alingments will apply to members of those classes for the next cycle

8. **Next cycle** (`i3jnext.sh $num $num+1`)
    - Makes the next cycle global average
    - the parameter file gets copied from `param-template.sh`
    - it is better to copy the last cycle param file again
    - start from step 2 again for the next cycle