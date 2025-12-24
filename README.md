# X-ALD Transcriptomic & Epigenetic Meta-Analysis
[![R](https://img.shields.io/badge/R-4.4.1-blue.svg)](https://www.r-project.org/)
[![Bioconductor](https://img.shields.io/badge/Bioconductor-3.20-green.svg)](https://bioconductor.org/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

This repository contains the R code for the meta-analysis of X-Linked Adrenoleukodystrophy (X-ALD). The study integrates transcriptomic data to identify core gene signatures and master regulator miRNAs.

## Datasets Used
1. **GSE117647**: Fibroblasts from X-ALD patients vs Controls.
2. **GSE34308**: Fibroblasts from X-ALD patients vs Controls.

## How to Run
1. Create a folder named `data`.
2. Inside `data`, create two subfolders: `GSE117647` and `GSE34308`.
3. Download the raw .CEL files from GEO (Gene Expression Omnibus) and place them in their respective folders.
4. Run `main_analysis.R` in RStudio.
