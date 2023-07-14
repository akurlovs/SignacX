[![CRAN Version](https://www.r-pkg.org/badges/version/SignacX)](https://cran.r-project.org/package=SignacX)
[![CRAN Downloads](https://cranlogs.r-pkg.org/badges/grand-total/SignacX)](https://cran.r-project.org/package=SignacX)

<!-- PROJECT LOGO -->
<br />
<p align="center">
  <h3 align="center">SignacX 2.2.4</h3>
  <p align="center">
    Get the most out of your single cell data.
    <br />
    <a href="https://sanofi-public.github.io/PMCB-SignacX/"><strong>Explore the Website »</strong></a>
    <br />
    <br />
    <a href="https://htmlpreview.github.io/?https://github.com/Sanofi-Public/PMCB-SignacX/tree/main/vignettes/signac-Seurat_pbmcs.html">View Demo</a>
    ·
    <a href="https://github.com/Sanofi-Public/PMCB-SignacX/">View Code Base</a>
    ·
    <a href="https://github.com/Sanofi-Public/PMCB-SignacX/issues">Request Feature</a>
  </p>
</p>

<!-- ABOUT THE PROJECT -->
#### What is SignacX?

SignacX is software that classifies the cellular phenotype for each individual cell in single cell RNA-sequencing data using neural networks trained with sorted bulk gene expression data from the [Human Primary Cell Atlas](https://bmcgenomics.biomedcentral.com/articles/10.1186/1471-2164-14-632). To learn more, check out the [publication](https://www.fortunejournals.com/abstract/cell-type-classification-and-discovery-across-diseases-technologies-and-tissues-reveals-conserved-gene-signatures-of-immune-phenot-4289.html) Cell Type Classification and Discovery across Diseases, Technologies and Tissues Reveals Conserved Gene Signatures of Immune Phenotypes), [website](https://sanofi-public.github.io/PMCB-SignacX/) and [code base](https://github.com/Sanofi-Public/PMCB-SignacX/). You can install SignacX from CRAN by running:

```R
install.packages("SignacX")
library(SignacX)
```

<!-- CONTACT -->
#### Contact

Andre Kurlovs - andre.kurlovs@sanofi.com
Nima Nouri -- nima.nouri@sanofi.com

<!-- NEWS -->
#### SignacX version history

##### SignacX 2.2.4 (2021-07-20) 

Enabled SignacX to classify datasets >300,000 cells -- fixed a memory allocation issue. First degree nearest KNN neighbors are now used for Shannon entropy calculation for datasets > 100,000 cells. 

##### SignacX 2.2.3 (2021-07-16) 

Fixed a typo in the help section for SignacX::MASC. 

##### SignacX 2.2.2
Addressed issues in the GitHub repository:
Labeling of individual cell states corresponding to broad cell types. 

##### SignacX 2.2.1
Addressed issues in the GitHub repository:
Integration with Seurat 4.0.0 clustering

##### SignacX 2.2.0 (2021-02-24) 

First CRAN release.
