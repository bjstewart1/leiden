# Leiden Algorithm

## leiden version 0.1.0

[![CRAN_Status_Badge](http://www.r-pkg.org/badges/version/leiden)](https://cran.r-project.org/package=leiden)
[![Travis Build Status](https://travis-ci.org/TomKellyGenetics/leiden.svg?branch=master)](https://travis-ci.org/TomKellyGenetics/leiden)
[![AppVeyor Build Status](https://ci.appveyor.com/api/projects/status/github/TomKellyGenetics/leiden?branch=master&svg=true)](https://ci.appveyor.com/project/TomKellyGenetics/leiden)
[![Project Status: Active – The project has reached a stable, usable state and is being actively developed.](http://www.repostatus.org/badges/latest/active.svg)](http://www.repostatus.org/#active)
[![codecov](https://codecov.io/gh/TomKellyGenetics/leiden/branch/master/graph/badge.svg)](https://codecov.io/gh/TomKellyGenetics/leiden)

## Clustering with the Leiden Algorithm in R

This package allows calling the Leiden algorthm for clustering on an igraph object from R. See the Python and Java implementations for more details: 

https://github.com/CWTSLeiden/networkanalysis

https://github.com/vtraag/leidenalg

## Install

This package requires the 'leidenalg' and 'igraph' modules for python (2) to be installed on your system. For example:

``pip install leidenalg igraph``

If you do not have root access, you can use `pip install --user` or `pip install --prefix` to install these in your user directory (which you have write permissions for) and ensure that this directory is in your PATH so that Python can find it.

The 'devtools' package will be used to install 'leiden' and the dependancies (igraph and reticulate):

```R
if (!requireNamespace("devtools"))
    install.packages("devtools")
devtools::install_github("TomKellyGenetics/leiden")
```

## Usage

This package provides a function to perform clustering with the Leiden algorithm:

```R
membership <- leiden(adjacency_matrix)
```

For an igraph object 'graph' in R:

```R
adjacency_matrix <- igraph::as_adjacency_matrix(graph)
membership <- leiden(adjacency_matrix)
```

To use Leiden with the Seurat pipeline for a Seurat Object `object` that has an SNN computed (for example with `Seurat::FindClusters` with `save.SNN = TRUE`). This will compute the Leiden clusters and add them to the Seurat Object Class.

```R
adjacency_matrix <- as.matrix(object@snn)
membership <- leiden(adjacency_matrix)
object@ident <- as.factor(membership)
names(test@ident) <- rownames(test@meta.data)
object@meta.data$ident <- as.factor(membership)
```

These clusters can then be plotted with:

```R
library("RColorBrewer")
colourPal <- function(groups) colorRampPalette(brewer.pal(min(length(names(table(groups))), 11), "Set3"))(length(names(table(groups))))

object <- RunPCA(object = object, do.print = TRUE, pcs.print = 1:5, genes.print = 5)
PCAPlot(object = object, colors.use = colourPal(object@ident), group.by = "ident")

object <- RunTSNE(object = object, dims.use = 1:20, do.fast = TRUE, dim.embed = 2)
TSNEPlot(object = object, colors.use = colourPal(object@ident), group.by = "ident")

object <- RunUMAP(object = object, dims.use = 1:20, metric = "correlation", max.dim = 2)
DimPlot(object, reduction.use = "umap", colors.use = colourPal(object@ident), group.by = "ident")
```


### Citation

Please cite this implementation R in if you use it:

```
To cite the leiden package in publications use:

  S. Thomas Kelly (2018). leiden: R implementation of the Leiden algorithm. R
  package version 0.1.0 https://github.com/TomKellyGenetics/leiden

A BibTeX entry for LaTeX users is

  @Manual{,
    title = {leiden: R implementation of the Leiden algorithm},
    author = {S. Thomas Kelly},
    year = {2018},
    note = {R package version 0.1.0},
    url = {https://github.com/TomKellyGenetics/leiden},
  }
 ```

Please also cite the original publication of this algorithm.

```
Traag, V.A., Waltman. L., Van Eck, N.-J. (2018). From Louvain to
       Leiden: guaranteeing well-connected communities.
       `arXiv:1810.08473 <https://arxiv.org/abs/1810.08473>`
```
