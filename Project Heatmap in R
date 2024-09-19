# Task Overview

In this task, we were expected to visualize and interpret a gene expression dataset to generate a heatmap and perform downstream functional enrichment analysis. The aim of this task was to help us understand and learn how to interpret patterns of gene expression and the biological significance of differentially expressed genes. The task involves data preprocessing, visualization, and interpretation of functional enrichment results.

## Dataset
Download and use one of these datasets:
- [Glioblastoma Dataset](https://raw.githubusercontent.com/HackBio-Internship/public_datasets/main/Cancer2024/glioblastoma.csv)
- [Pregnancy Lactation Cells Dataset](https://raw.githubusercontent.com/HackBio-Internship/public_datasets/main/Cancer2024/pregnancyLactationCells.csv)

## Tasks

1. **Heatmap Generation**:  
   - Generate a heatmap for the entire dataset.
   - Use a **diverging** and **sequential color palette** to generate two color variants of the same heatmap.
   - Discuss the importance of color selection in the ease of interpreting plots in your report.
   - We recommend using the `heatmap.2()` function from the `gplots` package in R.

2. **Heatmap Clustering Variants**:  
   - Generate variants of your heatmap where you:
     - Cluster genes (rows) alone.
     - Cluster samples (columns) alone.
     - Cluster both genes and samples together.

3. **Gene Subsetting**:
   - Subset genes that are significantly **upregulated** (set up your own cut-offs for fold change and p-values).
   - Subset genes that are significantly **downregulated** (set up your own cut-offs for fold change and p-values).

4. **Functional Enrichment Analysis**:
   - Perform functional enrichment analysis using tools like **ShinyGO**, **GOrilla**, or **PANTHER**.
   - Select the top 10 pathways and create a **visualization** (e.g., lollipop plot, dot plot, line plot, or bubble plot) displaying:
     - The number of genes associated with each pathway.
     - The significance of each pathway, scaled according to the negative log10 of the p-value.

5. **Reporting**:
   - Describe the top 3 enriched pathways based on biological processes in your report.
