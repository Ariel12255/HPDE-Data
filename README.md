# HPDE baseline epigenetic profiles at target genes — figures

Genome-browser-style chromatin profiles for the target genes (PDX1, NEUROG3, MAFA and their downstream genes) in the normal-derived human pancreatic ductal epithelial cell line HPDE6-E6E7, plus GAPDH and ACTB as positive controls. Pilot analysis on public data; the pipeline can be re-run on lab data.


## Folder contents

```
figures/
├── README.md
├── refstyle/                       <- main figures (style of Figure 3 in the reference paper)
│   ├── refstyle_overview.pdf/.png  all loci on one page (recommended starting point)
│   ├── refstyle_<GENE>.pdf/.png    one figure per locus (15 loci)
│   └── plot_refstyle.py            plotting script
├── tracks_<GENE>.pdf/.png          detailed tracks with MACS2 peak calls (black bars) and y-axis values
├── promoter_heatmap.pdf/.png       promoter (TSS ± 2.5 kb) signal per mark + state classification
├── promoter_summary.csv            numbers behind the heatmap
└── plot_tracks.py                  plotting script for tracks_* and the heatmap
```

Loci (hg38): PDX1, NEUROG3, MAFA, PAX6, NKX6-1, NEUROD1, PCSK1, PCSK2, ABCC8/KCNJ11 (one locus), KRT19, SOX9, FOXA2, INS/IGF2/TH, GAPDH, ACTB.

## How to read the figures

**refstyle figures** — tracks from top to bottom:

| Track | Colour | Meaning |
|---|---|---|
| H3K4me3 | green | active / poised promoter |
| H3K27ac | yellow | active promoter or enhancer |
| H3K4me1 | orange | enhancer (primed or active) |
| H3K27me3 | brown | Polycomb repression |
| ATAC-seq | navy | accessible (open) chromatin |

- Blue arrows = genes (direction of transcription); the target gene is in darker blue, and the shaded band marks the target gene ± 3 kb.
- Signal is smoothed CPM, averaged over replicates. Each mark uses the same scale across all loci, so peak heights can be compared between genes.
- `*` after a track name = replicate 1 only.

**tracks_ figures** — the same data unsmoothed, with numeric y-axes and MACS2 peaks drawn as black bars.

**promoter_heatmap** — mean CPM at TSS ± 2.5 kb (colour = z-score across genes; `*` = overlaps a peak). Promoter states:

| State | Rule (peaks with fold enrichment ≥ 2 and q < 0.01) |
|---|---|
| active | H3K4me3 and/or H3K27ac peak, no H3K27me3 peak |
| bivalent / poised | H3K4me3 or H3K27ac peak **and** H3K27me3 peak |
| Polycomb-repressed | H3K27me3 peak only |
| no mark | none of the above |

## Preliminary observations (baseline HPDE)

| State | Genes |
|---|---|
| Active | NKX6-1, FOXA2, SOX9, KRT19 (controls GAPDH, ACTB) |
| Bivalent / poised | PDX1, PAX6, KCNJ11 |
| Polycomb-repressed | NEUROG3, NEUROD1, PCSK1, PCSK2, ABCC8, MAFA |
| Unmarked / closed | INS |

## Data and methods

- **Source:** Zhong et al., *Gut* 2025 (doi:10.1101/2024.09.22.24314165); SRA BioProject PRJNA1041452. HPDE6-E6E7: H3K4me3, H3K27ac, H3K4me1, H3K27me3 ChIP-seq and 2% input (2 replicates each, single-end 75 bp) and ATAC-seq (2 replicates, paired-end 150 bp).
- **Processing (Galaxy, usegalaxy.org)
- **Plotting:** Python 3 (pandas, NumPy, matplotlib).

## Caveats

- Plotted windows are 30–150 kb. They will be widened to whole gene ± 50–100 kb to capture all H3K27me3 peaks across each locus.
- ATAC-seq currently uses replicate 1 only; ATAC peaks are pending.
- No DNA methylation data are available for HPDE in this dataset; a methylation track still needs a source.
- HPDE6-E6E7 is an immortalised line and may differ from primary ductal cells.

## Next steps

1. Widen windows and quantify H3K27me3 per locus (number of peaks, % of locus covered, total signal).
2. Relate each downstream gene's epigenetic state to its induction level after CRISPRa of PDX1 / NEUROG3 / MAFA.
3. Add a DNA methylation track.
