# X-ALD Transcriptomic & Epigenetic Meta-Analysis
[![R](https://img.shields.io/badge/R-4.4.1-blue.svg)](https://www.r-project.org/)
[![Bioconductor](https://img.shields.io/badge/Bioconductor-3.20-green.svg)](https://bioconductor.org/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

This repository contains the R code for the meta-analysis of X-Linked Adrenoleukodystrophy (X-ALD). The study integrates transcriptomic data to identify core gene signatures and master regulator miRNAs.

## Datasets Used
1. **GSE117647**: Fibroblasts from X-ALD patients vs Controls.
2. **GSE34308**: Fibroblasts from X-ALD patients vs Controls.

### Key Capabilities

- Single and multi-dataset comparisons
- Conservative (adj.P < 0.05) and liberal (P < 0.05) DEG thresholds
- Comprehensive miRNA target prediction (validated & predicted targets)
- Cross-species orthology analysis
- Built-in error handling for large-scale analyses

### Human Datasets (Extension Support)
- **Species**: *Homo sapiens*
- **Platform**: Affymetrix Human Genome U133 Plus 2.0 Array
- **Status**: Pipeline ready, awaiting human X-ALD datasets

  ## 🔬 Methodology

### Data Processing Pipeline

1. **Quality Control & Normalization**
   - CEL file reading with `oligo` package
   - RMA normalization
   - Quality metrics assessment
   - Sample selection optimization

     2. **Differential Expression Analysis**
   - LIMMA statistical framework with empirical Bayes moderation
   - Multiple testing correction (Benjamini-Hochberg FDR)
   - **Dual threshold strategies**:
     - **Conservative**: adj.P < 0.05, |logFC| > 0.5 and 0.25 
     - **Liberal**: P.Value < 0.05 (for cross-dataset common DEGs)
   - Batch effect control in multi-dataset analyses

  3. **Functional Enrichment Analysis**
   - Gene Ontology (GO) enrichment (BP, MF, CC)
   - KEGG pathway analysis
   - Over-Representation Analysis (ORA) using `clusterProfiler`
   - Metabolic pathway-focused analysis

     4. **miRNA Target Prediction**
   - **multiMiR database integration** (11 comprehensive databases):
     - miRecords, miRTarBase, TarBase (validated targets)
     - DIANA-microT, ElMMo, MicroCosm, miRanda, miRDB, PicTar, PITA, TargetScan (predicted targets)
   - **Chunking strategy** for large gene lists (>4000 genes) to avoid Gateway Timeout
   - Target prioritization algorithms with balanced scoring
   - Network analysis and visualization

     ### System Prerequisites

####  R & RTools
- **R** version 4.4.x or newer
- **RTools** version 4.4 (or corresponding version for your R)
- **⚠️ CRITICAL**: RTools must be installed and properly configured:
  - `make` compiler must be in system PATH
  - `gcc`/`g++` compilers must be in system PATH
  - Required for building packages from source
  - Download: https://cran.r-project.org/bin/windows/Rtools/

 ### R Package Installation

#### Bioconductor Packages
```r
# Install BiocManager if not already installed
if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")

#### CRAN Packages
```r
# Install tidyverse bundle (includes dplyr, ggplot2, readr)
install.packages("tidyverse")

# Additional visualization
install.packages(c(
  "ggrepel",                           # Label repelling in plots
  "pheatmap"                           # Heatmap visualization
))
```

### Running the Analysis

1. **Group Correction** (if needed):
```bash
Rscript scripts/00_CORRECTION_working.R
```

2. **Quality Control**:
```bash
Rscript scripts/01_quality_control.R
```

3. **Differential Expression Analysis**:
```bash
Rscript scripts/02_differential_expression.R
```

4. **Pathway and miRNA Analysis**:
```bash
Rscript scripts/03_pathway_mirna_analysis.R
```

### Quick Start

```r
# Load the main results
load("results/differential_expression_workspace.RData")

# View significant genes
significant_genes <- read.csv("results/significant_genes.csv")
head(significant_genes)

# View pathway results
pathway_summary <- read.delim("results/pathway_analysis/FINAL_ANALYSIS_SUMMARY.txt")
```

## 📈 Statistical Methods

- **Normalization**: Robust Multi-Array Average (RMA)
- **Differential Expression**: Linear Models for Microarray Data (LIMMA) with empirical Bayes
- **Multiple Testing Correction**: Benjamini-Hochberg False Discovery Rate (FDR)
- **Pathway Enrichment**: Over-Representation Analysis (ORA) using `clusterProfiler`
- **miRNA Prediction**: multiMiR integration (8 databases: validated + predicted targets)
- **Batch Effect Control**: Covariate adjustment in design matrix

  ## 🛠️ Technical Specifications

### Software Environment
- **R Version**: 4.4.1 or newer
- **Bioconductor Version**: 3.20 or newer
- **RTools**: 4.4 (required for package compilation)
- **Operating Systems**: Windows, macOS, Linux

### Array Platforms Supported
- **Human**: Affymetrix Human Genome U133 Plus 2.0 Array

### Annotation Packages
- **Human**: `hgu133plus2.db`, `org.Hs.eg.db`

### Database Resources
- **multiMiR**: 8 integrated miRNA-target databases
- **miRBase**: mature.fa for sequence-based ortholog matching
- **KEGG**: Pathway database ( human: hsa)
- **Gene Ontology**: BP, MF, CC ontologies

    ## ⚙️ Built-in Error Handling

The pipeline includes robust error handling mechanisms:

### 1. Deprecated Package Bypass
- **Issue**: `pd.hgu133plus2` package deprecation warnings
- **Solution**: Automatic bypass using alternative annotation methods
- **Impact**: Seamless human dataset processing

### 2. Gateway Timeout Prevention (multiMiR)
- **Issue**: multiMiR server timeouts with large gene lists (4000+ genes)
- **Solution**: Automatic "chunking" - queries split into manageable batches
- **Implementation**: Sequential batch processing with result aggregation
- **Impact**: Reliable miRNA prediction for any dataset size

### 3. S4 Object Merge Conflicts
- **Issue**: multiMiR returns S4 objects with mismatched columns across chunks
- **Solution**: `dplyr::bind_rows()` with column harmonization
- **Impact**: Clean result aggregation from chunked queries

### 4. Package Namespace Conflicts
- **Issue**: Function masking between packages (e.g., `dplyr::select` vs `S4Vectors::select`)
- **Solution**: Explicit namespace calls (`dplyr::rename`, `dplyr::select`)
- **Impact**: Prevents unexpected function behavior and errors

### 5. Memory Management
- **Strategy**: Incremental processing for large datasets
- **Chunking**: Configurable batch sizes for multiMiR queries
- **Cleanup**: Explicit garbage collection after large operations

  #### miRNA Therapeutic Potential
Identified miRNAs represent:
- **Therapeutic targets**: Antisense oligonucleotide (ASO) or small molecule interventions
- **Diagnostic biomarkers**: Disease diagnosis and severity monitoring
- **Prognostic indicators**: Treatment response and disease progression
- **Mechanistic insights**: Understanding MMA pathophysiology at post-transcriptional level

### Databases & Tools
- **multiMiR** (v1.x): Comprehensive miRNA-target database integration
  - 8 databases: miRecords, miRTarBase, TarBase, DIANA-microT, ElMMo, MicroCosm, miRanda, miRDB, PicTar, PITA, TargetScan
  - Ru et al. (2014). The multiMiR R package and database. *Journal of Clinical Bioinformatics*.
- **clusterProfiler** (v4.x): Pathway enrichment analysis
  - Yu et al. (2012). clusterProfiler: an R package for comparing biological themes among gene clusters. *OMICS*.
- **LIMMA** (v3.x): Differential expression analysis
  - Ritchie et al. (2015). limma powers differential expression analyses. *Nucleic Acids Research*.
- **KEGG**: Kyoto Encyclopedia of Genes and Genomes
- **Gene Ontology Consortium**: GO annotations (BP, MF, CC)

### Methodological References
- RMA Normalization: Irizarry et al. (2003). *Biostatistics*.
- Benjamini-Hochberg FDR: Benjamini & Hochberg (1995). *Journal of the Royal Statistical Society*.
- Over-Representation Analysis: Rivals et al. (2007). *PLoS ONE*.

  ## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

**Data Availability**: Public (GEO Accession: GSE117647, GSE34308)  
**Reproducibility**: Full scripts and data provided

## How to Run
1. Create a folder named `data`.
2. Inside `data`, create two subfolders: `GSE117647` and `GSE34308`.
3. Download the raw .CEL files from GEO (Gene Expression Omnibus) and place them in their respective folders.
4. Run `main_analysis.R` in RStudio.
