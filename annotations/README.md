# Annotation tables

Pacific oyster (*Crassostrea gigas*, syn. *Magallana gigas*) single-cell atlas, genome xbMagGiga1.1 (GCF_963853765.1), RefSeq RS_2024_06. Maps and clusters from Seurat.
Licence: CC BY 4.0.

| File | Rows | What |
|---|---:|---|
| `cells.tsv.gz` | 186,414 | one row per cell |
| `cell_types.tsv` | 213 | one row per cell type |
| `families.tsv` | 80 | cell types in each of the 22 cross-dataset families |
| `family_matches.tsv` | 90 | robust cross-dataset cell-type matches |
| `libraries.tsv` | 20 | sequencing runs behind the `library` column |
| `marker_db_v1.xlsx` | | published markers translated to current gene IDs, and top genes of every cell type |

## Columns

`cells.tsv.gz`
- `cell`: library name + `_` + cell barcode as in the original reads
- `atlas`: development, hemocyte, priming, gonad, ganglia, mantle
- `dataset`: early_embryo (cleavage and blastula), gastrula, trochophore, or the atlas name
- `stage`: developmental stage (development atlas only)
- `cluster`: the cluster unit the adult cell-type names were assigned to; development keeps the authors' clusters
- `seurat_cluster`: Seurat cluster (Louvain, resolution 1) within the atlas
- `umap_x`, `umap_y`: Seurat UMAP of the atlas (RPCA-integrated for gonad and ganglia, uncorrected otherwise)
- `cell_type`: cell-type name. Development uses the authors' names; adult atlases use our evidence-graded names, with the authors' name in brackets where it differs
- `lineage`: hemocyte, neural, epithelial/secretory, connective/muscle, germline, gonad somatic, embryonic/other, unresolved
- `confidence`: author (authors' label), high, medium, low, unresolved, transfer (hemocyte priming: transferred from the hemocyte atlas)

`cell_types.tsv`: `short_name` is the name shown on the website; `family` is empty when no robust match exists;
`in_web` is false for 4 cell types with fewer than 20 cells, which the website does not show.

`family_matches.tsv`: symmetric AUROC from the MetaNeighbor R package (MetaNeighborUS, fast version) on Seurat-normalised data and
3,426 variable genes (Seurat vst, variable in at least two datasets). A match is a pair of reciprocal top hits with AUROC at least 0.9 and
one-vs-best AUROC at least 0.6. The table lists robust pairs: matched in at least 6 of 9 re-runs with different cell samples and gene sets
(`stable_in_runs_of_9`); `base_run_match` says whether the pair also matched in the base run (90 of 90).
Families are connected components of the robust pairs (22 families, 13 span three or more datasets).
Matches are transcriptional similarity, not lineage.

Raw reads are not redistributed; use the accessions in `libraries.tsv`.
Preliminary release; the manuscript is in preparation and names may change before publication.
