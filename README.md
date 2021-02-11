[![Contributors][contributors-shield]][contributors-url]
[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]
[![GPL License][license-shield]][license-url]
[![LinkedIn][linkedin-shield]][linkedin-url]

<!-- PROJECT LOGO -->
<br />
<p align="center">
  <a href="https://www.sanofi.com/">
    <img src="images/GitHubFigure.svg" alt="Logo" width="600" height="300">
  </a>

  <h3 align="center">Signac 2.1.0</h3>

  <p align="center">
    Get the most out of your single cell data.
    <br />
    <a href="#getting-started"><strong>Explore the docs »</strong></a>
    <br />
    <br />
    <a href="https://htmlpreview.github.io/?https://github.com/mathewchamberlain/Signac/master/vignettes/signac-Seurat_CITEseq.html">View Demo</a>
    ·
    <a href="https://github.com/mathewchamberlain/Signac/issues">Report Bug</a>
    ·
    <a href="https://github.com/mathewchamberlain/Signac/issues">Request Feature</a>
  </p>
</p>



<!-- TABLE OF CONTENTS -->
## Table of Contents

* [What is Signac?](#about-the-project)
* [Getting Started](#getting-started)
  * [Installation](#installation)
  * [Quick start](#quickstart)
* [Usage](#usage)
  * [SPRING](#spring)
  * [Seurat](#seurat)
  * [Non-human data](#non-human-data)
  * [Genes of interest](#genes-of-interest)
  * [Learning from single cell data](#learning-from-single-cell-data)
* [Benchmarking](#benchmarking) 
  * [CITE-seq](#cite-seq-pbmcs)
  * [Flow cytometry synovium](#flow-cytometry-synovium)
  * [Pbmc Bench](#pbmc-bench)
* [Roadmap](#roadmap)
* [Contributing](#contributing)
* [License](#license)
* [Contact](#contact)

<!-- ABOUT THE PROJECT -->
## What is Signac?

Signac is software developed and maintained by the Savova lab at Sanofi with a focus on single cell genomics for clinical applications. Signac helps solve the cell type classification problem in single cell RNA sequencing: We have gene expression profiles for each individual cell, but we do not know the cellular phenotypes. Signac classifies the cellular phenotype for each individual cell in single cell data using neural networks trained with sorted bulk gene expression data from the [Human Primary Cell Atlas](https://bmcgenomics.biomedcentral.com/articles/10.1186/1471-2164-14-632). We demonstrate that Signac can:

* Classify cellular phenotypes in single cell data derived from any disease, tissue or technology.
* Classify non-human single cell data
* Identify potentially novel cellular populations (avoiding known single-cell artifacts)
* Learn from single cell data to map cellular phenotype labels across single cell data sets. 

Finally, we used Signac annotations to identify potential opportunities for drug targets for rheumatoid arthritis using a precision-medicine strategy. Check out the pre-print [here](https://www.biorxiv.org/content/10.1101/2021.02.01.429207v2).

<!-- GETTING STARTED -->
## Getting Started

To make life easier, Signac was integrated with [Seurat](https://satijalab.org/seurat/) (versions 3 and 4), [MASC](https://pubmed.ncbi.nlm.nih.gov/30333237/) and [SPRING](https://pubmed.ncbi.nlm.nih.gov/29228172/), allowing for straightforward visualization and analysis of single cell data using existing tools.

To install Signac in R, simply do:

### Installation

```r
devtools::install_github("mathewchamberlain/Signac")
```

### Quick start

The main functions in Signac are:

```r
# load the library
library(Signac)

# Load the reference training data set
data("training_HPCA")

# Generate initial labels
labels = Signac(your_data_here, R = training_HPCA)

# Get cell type labels
celltypes = Generate_lbls(labels, your_data_here)
```

<!-- USAGE EXAMPLES -->
## Usage

Running Signac is simple. We provide a few vignettes:

### SPRING
In the [pre-print](https://www.biorxiv.org/content/10.1101/2021.02.01.429207v3), we often used Signac integrated with [SPRING](https://pubmed.ncbi.nlm.nih.gov/29228172/), which let us easily explore cellular phenotypes identified by Signac interactively using the SPRING Viewer. To reproduce our findings and to generate new results with SPRING, please visit our repository which has [example notebooks for processing CITE-seq and scRNA-seq data from 10X Genomics in SPRING and integration with Signac](https://github.com/mathewchamberlain/SPRING_dev/). Briefly, Signac is integrated seamlessly with the output files of SPRING in R, like so:

```r
# load the Signac library
library(Signac)

# dir points to the "FullDataset_v1" directory generated by the SPRING Jupyter notebook
dir = "./FullDataset_v1" 

# load the expression data
E = CID.LoadData(dir)

# load the reference data
data("training_HPCA")

# generate cellular phenotype labels
labels = Signac(E, R = training_HPCA, spring.dir = dir)
celltypes = Generate_lbls(labels, E = E)

# write cell types and Louvain clusters to SPRING
dat <- CID.writeJSON(celltypes, data.dir = dir)
```

After running these functions with Signac, cellular phenotypes and Louvain clusters are ready to be visualized with SPRING Viewer, which can be setup locally as described [here](https://github.com/mathewchamberlain/SPRING_dev/).

### Seurat
Another way to use Signac is with Seurat. [In this vignette](https://htmlpreview.github.io/?https://github.com/mathewchamberlain/Signac/master/vignettes/signac-Seurat_CITEseq.html), we performed multi-modal analysis of CITE-seq PBMCs from 10X Genomics using Seurat integrated with Signac. Note: This same data set was also processed using SPRING [with this notebook](https://github.com/mathewchamberlain/SPRING_dev/blob/master/data_prep/spring_notebook_10X_CITEseq.ipynb). 

Sometimes, we have single cell genomics data with disease information. In [this vignette](https://htmlpreview.github.io/?https://github.com/mathewchamberlain/Signac/master/vignettes/signac-Seurat_AMP.html), we applied Signac to healthy and lupus nephritis kidney cells, and then used MASC to identify disease-enriched cellular phenotypes.

### Non-human data

In Supplemental Figure 8 of the [pre-print](https://www.biorxiv.org/content/10.1101/2021.02.01.429207v3), we classified single cell data for a model organism (cynomolgus monkey) for which flow-sorted datasets were generally lacking without any additional species-specific training. Instead, we mapped homologous genes from the *Macaca fascicularis* genome to the human genome in the single cell data, and then performed cell type classification with Signac. We demonstrate how we mapped the gene symbols [here](https://htmlpreview.github.io/?https://github.com/mathewchamberlain/Signac/master/vignettes/Crabeating_vignette.html). Note: this code can be used for to identify homologous genes between any two species.

### Genes of interest

In Figure 6 of the [pre-print](https://www.biorxiv.org/content/10.1101/2021.02.01.429207v3), we compiled data from three source (CellPhoneDB, GWAS catalog and the priority index paper) to find genes of immunological / pharmacological interest. These genes can be accessed internally from within Signac:

```r
# load the library
library(Signac)

# See ?Genes_Of_Interest
data("Genes_Of_Interest")
```

### Learning from single cell data

In Figure 4 of the [pre-print](https://www.biorxiv.org/content/10.1101/2021.02.01.429207v3), we demonstrated that we could train a model to recognize cell types in one single cell data set, and then use the model to classify cells in other data sets (allowing Signac to map cells or clusters from one single cell data set to another). Particularly, we identified CD56<sup>bright</sup> NK cells from CD56 and CD16 protein expression with CITE-seq data, and then identify similar cells in other single cell data sets, labeling them "CD56<sup>bright</sup> NK cells." [Here, we provide a vignette](https://htmlpreview.github.io/?https://github.com/mathewchamberlain/Signac/master/vignettes/signac-SPRING_Learning.html) for reproducing this analysis. Note: the data used here were processed with SPRING prior to classification with Signac; those notebooks are available [here](https://github.com/mathewchamberlain/SPRING_dev/).

<!-- BENCHMARKING -->
## Benchmarking

Although there are many automatic cell type annotation methods available, surprisingly they have not been validated with flow cytometry or CITE-seq empirical data. Furthermore, [in a benchmarking study](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-019-1795-z), all methods performed poorly in inter-dataset classification (i.e., each method exhibited technology- or tissue-specific performance bias), and no prior-knowledge classifiers performed well across data derived from PBMCs from any technology. Thus, we set out to benchmark Signac:

### CITE-seq PBMCs

We found high agreement of Signac cell type annotations (generated using only gene expression data) with cell-type specific protein expression data in CITE-seq PBMCs, whether the data were analyzed with SPRING-Signac ([here](https://github.com/mathewchamberlain/SPRING_dev/)) or with Seurat-Signac ([here](https://htmlpreview.github.io/?https://github.com/mathewchamberlain/Signac/master/vignettes/signac-Seurat_CITEseq.html)), indicating that Signac identified immune cell phenotypes consistent with empirical data.

### Flow cytometry synovium

Next, we found high agreement between cell types identified with flow cytometry and those generated by Signac. 

### PBMCs technology benchmarking



<!-- ROADMAP -->
## Roadmap

See the [open issues](https://github.com/mathewchamberlain/Signac/issues) for a list of proposed features (and known issues).

<!-- CONTRIBUTING -->
## Contributing

Contributions are what make the open source community such an amazing place to be learn, inspire, and create. Any contributions you make are **greatly appreciated**.

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

<!-- LICENSE -->
## License

Distributed under the GPL v3.0 License. See `LICENSE` for more information.

<!-- CONTACT -->
## Contact

Mathew Chamberlain - [linkedin](https://linkedin.com/in/chamberlainmathew) - mathew.chamberlain@sanofi.com

Project Link: [https://github.com/mathewchamberlain/Signac](https://github.com/mathewchamberlain/Signac)

<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->
[contributors-shield]: https://img.shields.io/github/contributors/mathewchamberlain/Signac.svg?style=flat-square
[contributors-url]: https://github.com/mathewchamberlain/Signac/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/mathewchamberlain/Signac.svg?style=flat-square
[forks-url]: https://github.com/mathewchamberlain/Signac/network/members
[stars-shield]: https://img.shields.io/github/stars/mathewchamberlain/Signac.svg?style=flat-square
[stars-url]: https://github.com/mathewchamberlain/Signac/stargazers
[issues-shield]: https://img.shields.io/github/issues/mathewchamberlain/Signac.svg?style=flat-square
[issues-url]: https://github.com/mathewchamberlain/Signac/issues
[license-shield]: https://img.shields.io/github/license/mathewchamberlain/Signac.svg?style=flat-square
[license-url]: https://choosealicense.com/licenses/gpl-3.0/
[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-black.svg?style=flat-square&logo=linkedin&colorB=555
[linkedin-url]: https://linkedin.com/in/chamberlainmathew
[product-screenshot]: images/screenshot.png
[![Downloads](https://img.shields.io/github/downloads/Cigaras/IPTV.bundle/total.svg "Downloads")](https://github.com/Cigaras/IPTV.bundle/releases)
