<img src="vignettes/logo_large_hexagon.gif" height="135" width="135" style="align:center" />

------------------------------------------------------------------------------------
Fast and efficient summarization of generic bedGraph files from Bisufite sequencing
------------------------------------------------------------------------------------

<!-- badges: start -->

[![CRAN status](https://www.r-pkg.org/badges/version/methrix)](https://cran.r-project.org/package=methrix)
[![Travis build status](https://travis-ci.com/CompEpigen/methrix.svg?branch=master)](https://travis-ci.com/CompEpigen/methrix)
[![Lifecycle: maturing](https://img.shields.io/badge/lifecycle-maturing-blue.svg)](https://www.tidyverse.org/lifecycle/#maturing)
[![BioC status](http://www.bioconductor.org/shields/build/release/bioc/methrix.svg)](https://bioconductor.org/checkResults/release/bioc-LATEST/methrix)

<!-- badges: end -->

### Introduction

Bedgraph files generated by BS pipelines often come in various flavors. Critical downstream step requires aggregation of these files into methylation/coverage matrices. This step of data aggregation is done by Methrix, including many other useful downstream functions.

<p align="center">
<img src="https://github.com/CompEpigen/methrix/blob/master/vignettes/overview.png">
</p>

### Summary:

* Faster summarization of generic bedGraph files with `data.table` back-end
* Fills missing CpGs from reference genome
* Vectorized code (faster, memory expensive) and non-vectorized code (slower, minimal memory)
* Built upon `SummarizedExperiment` with custom methods for CpG extraction, sub-setting, and filtering
* Easy conversion to bsseq object for downstream analysis
* Extensive one click interactive html report generation
* Supports serialized arrays with `HDF5Array` and `saveHDF5SummarizedExperiment`

### Updates:
see [here](https://github.com/CompEpigen/methrix/blob/master/NEWS.md)

### Installation
```r
if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")
BiocManager::install("CompEpigen/methrix")
```

### Usage:
#### Step-1: Extract all CpG loci from the reference genome

```r
> hg19_cpgs = methrix::extract_CPGs(ref_genome = "BSgenome.Hsapiens.UCSC.hg19")
# Extracting CpGs..
# Here is a Chuck Norris joke while you wait..
# Chuck Norris doesn't chew gum. Chuck Norris chews tin foil.
# Done.
# Extracted 28787054 from 86 contigs.
```
#### Step-2: Read in bedgraphs and generate a methrix object

Test data is 3 stranded WGBS bedgraphs from MethylcTools

```r
#Example bedgraph files
> bdg_files = list.files(path = system.file('extdata', package = 'methrix'), pattern = "*bdg\\.gz$", full.names = TRUE)

> meth = methrix::read_bedgraphs(files = bdg_files, ref_cpgs = hg19_cpgs, chr_idx = 1, start_idx = 2, M_idx = 3, U_idx = 4,
  stranded = TRUE, collapse_strands = TRUE)
----------------------------
-Preset:        Custom 
--Missing beta and coverage info. Estimating them from M and U values
-CpGs raw:      28,700,086
-CpGs filtered: 28,217,448
-CpGs stranded: 56,434,896
----------------------------
-Batch:         1/1 
-Processing:    C1.bdg.gz
--CpGs missing: 56,434,254
-Processing:    C2.bdg.gz
--CpGs missing: 56,434,231
-Processing:    N1.bdg.gz
--CpGs missing: 56,434,233
-Processing:    N2.bdg.gz
--CpGs missing: 56,434,233
-Finished in:   00:01:19 elapsed (00:01:52 cpu) 

> meth
An object of class methrix
   n_CpGs: 28,217,448
n_samples: 4
    is_h5: FALSE
Reference: Genome Reference Consortium GRCh37
```

### Methrix operations

What can be done on `methrix` object? Following are the key functions

```r
subset_methrix()    #Subset methrix object based on given condition
write_bedgraphs()   #Writes bedGraphs from methrix object
order_by_sd()       #order methrix object by SD
methrix_report()    #Create a detailed interative html summary report from methrix object
methrix2bsseq()     #Convert methrix to bsseq object
region_filter()	    #Filter matrices by region
remove_uncovered()	#Remove loci that are uncovered across all samples
coverage_filter()   #Filters methrix object based on coverage
methrix_pca()	      #Principal Component Analysis
subset_methrix	    #Subsets 'methrix' object based on given conditions.
get_stats()	        #Estimate descriptive statistics of the object
methrix_report()	  #Creates a detailed interative html summary report from Methrix object
```
plus other plotting functions..

### Speed benchmark

To be added

### To do

- [X] Write methods
- [X] Add vignette
- [X] Add examples
- [ ] Clear BiocCheck warnings and notes
- [ ] Submit to BioConductor for Fall (3.10) release


### Note

***This repository is under active development***
