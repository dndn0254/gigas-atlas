# Gigas Cell Atlas

A uniformly reprocessed single-cell atlas of the Pacific oyster (*Crassostrea gigas*): 186,414 cells from 8 public
single-cell and single-nucleus RNA-seq studies (embryo, larva, blood, gonad, ganglia, mantle), rebuilt from raw reads on the current
genome (xbMagGiga1.1 (GCF_963853765.1), RefSeq RS_2024_06), analysed with Seurat and named the same way.

**Website**: https://dndn0254.github.io/gigas-atlas/

| | |
|---|---|
| `index.html` | project page |
| `explorer/` | look up a gene or a cell type, test a gene list, convert old Roslin v1 gene IDs |
| `families.html` | the same cell type found again in other datasets (22 families, 90 robust matches) |
| `annotations/` | cell-level and cell-type-level tables, marker database ([details](annotations/README.md)) |

The website is static HTML and JavaScript; it runs entirely in the browser and sends nothing anywhere.

Name the cells of new oyster data with these references, in R on a Seurat object or in Python: https://github.com/dndn0254/gigasatlas

## How it was made

1. Public raw reads (27 SRA runs, 20 libraries) aligned to xbMagGiga1.1 (GCF_963853765.1, RefSeq annotation RS_2024_06) with
   STARsolo 2.7.11b, Cell Ranger-matched settings, EmptyDrops_CR cell calling.
2. Cell QC per library (median minus 3 MAD of log UMI and log genes); doublets removed when 2 of 3 tools agree (Scrublet, scDblFinder, SOLO).
3. Seurat 5.5.1 (R 4.5.3) per atlas: NormalizeData (LogNormalize), FindVariableFeatures (vst, 3,000), ScaleData, RunPCA (50),
   IntegrateLayers with RPCA for the gonad and ganglia atlases (several libraries of one tissue), FindNeighbors (k 20), FindClusters (Louvain), RunUMAP.
4. Cell-type names graded by evidence (authors' markers, literature markers, cluster genes, hemocyte programs, QC flags).
5. Cross-dataset matching with MetaNeighbor 1.29.0 (MetaNeighborUS) on Seurat-normalised data: reciprocal top hits, AUROC at least 0.9,
   one-vs-best AUROC at least 0.6, stable in at least 6 of 9 re-runs: 90 pairs, 22 families.

## Source studies

Gavery et al. 2025 (PRJNA909306); Piovani et al. 2023 (PRJNA967338); de La Forest Divonne et al. 2025 (PRJEB74031);
PRJNA1236035 (no paper); Wang et al. 2024 and 2025 (PRJNA983861, PRJNA1244049, PRJNA1347576); Yang et al. 2026 (PRJNA1457765);
Bai et al. 2026, preprint (PRJNA1338802). Please cite them when you use their cells.

## Status and citation

Preview release (v5 2026-11-12 (atlas v4 cells, Seurat 5.5.1 maps, MetaNeighbor R families)). The manuscript is in preparation; numbers and names may change before publication.
Until then, cite this repository.

## Licence

Data and text (annotations, website content): CC BY 4.0. Website code: MIT. See `LICENSE`.
