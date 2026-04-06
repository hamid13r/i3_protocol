# i3 Protocol Cycle Steps

Every cycle of i3 goes through the following steps:

1. **Subvolume Extraction and Averaging** (`i3jinitial.sh` or `i3jnext.sh`)
    - Subvolumes are cut out and averaged using the initial alignment or the alignment from the last cycle

2. **Reference Selection** (`i3jreference.sh`)

3. **Multi-Reference Alignment (MRA)**
    - Subvolumes are aligned to the reference
    - Configurable parameters:
      - Binary alignment mask (soft mask required) for region-specific alignment
      - Filtering: Low-pass and high-pass Fourier frequencies in x, y, z (0.5 = full resolution, 0.25 = 2× pixel size)
      - Search ranges: `-dx` to `+dx` in x, y, z (cannot be zero; use 0.1 to skip)
      - Angular search: `cone-range cone-sampling rotation-range rotation-sampling`
    - Note: Sometimes skipped due to reference bias susceptibility

4. **Multivariate Statistical Analysis (MSA)** (`i3jmsa.sh`)
    - Uses principal component analysis to identify dataset heterogeneities
    - Optional mask for region-specific analysis
    - Eigen-vectors saved as `cycle-???/something.rsv`
    - **Factor 0**: Global average
    - **Factors 1–2**: Typically represent missing-wedge artifacts
    - **Remaining factors**: Detected heterogeneities (may have inverted contrast)

5. **Classification**
    - Projects dataset onto selected factors
    - Supports multiple class ranges (e.g., 2–20 with step of 2 produces: 2, 4, 8, …, 20)
    - Subset selection possible (e.g., 2, 4, 10)

6. **Copy Selected Classification** (`i3jcp.sh`)
    - Choose optimal classification balancing heterogeneity vs. particle count

7. **Class-Average Alignment**
    - Align class-averages to each other

