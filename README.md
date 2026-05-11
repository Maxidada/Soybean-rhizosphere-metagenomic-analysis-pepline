# Soybean-rhizosphere-metagenomic-analysis-pepline


This repository contains a reproducible computational pipeline for analyzing soybean rhizosphere metagenomic sequencing data. The workflow integrates:
Quality control & host read removal (fastp, Bowtie2)
Taxonomic profiling (Kraken2, Bracken)
Community ecology analysis (PCoA, PERMANOVA, Shannon diversity)
Metagenome assembly (MEGAHIT, QUAST)
Functional annotation (KEGG KO HMM profiles, HMMER, Prodigal)
Gene abundance quantification (TPM normalization)
Publication-ready visualizations (ggplot2, patchwork)
The pipeline is designed for HPC environments using SLURM job scheduling and supports parallel processing for scalability.


#Step-by-Step Description

QC & Host Removal (01_QC_HostRemoval.sh)
Merge technical replicates → fastp QC → Bowtie2 mapping to soybean genome → retain unmapped reads
Taxonomic Classification (02_Kraken2_Bracken.sh)
Kraken2 classification (confidence ≥ 0.1) → Bracken abundance re-estimation at 6 taxonomic levels (S/G/F/O/C/P)
Abundance Matrix Generation (03_Merge_Bracken.py)
Merge per-sample Bracken outputs → sample × taxon matrices using fraction_total_reads
Community Ecology Analysis (04_PCoA_analysis.R)
Hellinger transformation → Bray-Curtis dissimilarity → PCoA → PERMANOVA + PERMDISP tests
Composition Visualization (05_*.R, 06_*.R)
Stacked bar plots of top-20 families/genera with QIIME2-style color palette
Alpha Diversity (07_Shannon_Diversity.R)
Genus-level Shannon index calculation + genotype comparison (t-test) with significance stars
Read Normalization (08_BBNorm_Normalization.sh)
BBnorm target depth = 100×, min depth = 5× for even coverage prior to assembly
Metagenome Assembly (09_MEGAHIT_Assembly.sh)
Co-assembly of all normalized reads → QUAST quality metrics (N50, # contigs, etc.)
Functional Database Preparation (10_*.sh – 14_*.sh)
Download KO amino acid sequences → CD-HIT clustering (99% ID) → MAFFT alignment → trimAl → hmmbuild → merged HMM database
Gene Abundance Quantification (15_TPM_Calculation.sh)
Prodigal gene prediction → HMMER search (E ≤ 1e-10) → Bowtie2 read mapping → TPM calculation
Functional Annotation & Visualization (16_*.sh, 17_*.R)
Map genes to KOs → generate KO-tagged TPM matrix → heatmap of nutrient-related gene expression across treatments


#Citation
@article{wang2026soybean,
  title = {Phosphate-dependent remodeling of the soybean rhizosphere microbiome},
  author = {Wang, Qi and Garrido-Oter, Ruben and Hacquard, St{\'e}phane and Schulze-Lefert, Paul},
  journal = {Plant Microbe Interactions},
  year = {2026},
  publisher = {Max Planck Institute for Plant Breeding Research}
}

#Contact

Qi Wang
Max Planck Institute for Plant Breeding Research
📧 qwang2@mpipz.mpg.de
