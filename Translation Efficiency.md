# Translation Efficiency Module

**Translation Efficiency** is a methodology to see differentialy expressed genes on different conditions. The difference between **Differential Expression (DE)** and **Translation Efficiency (TE)** is the formulation of generalized linear model. While in DE analyses model is built up on the difference between two *conditions* (Control-Treatment) in specific *sequencing type* (RiboSeq or RNASeq), in the TE the model built up on the intersection terms of sequencing type and conditions. As further explanation since number of RPFs on a gene is highly influenced by the number of mRNA copies of that gene, doing only DE on RiboSeq may not reflect the real differential production of that specific gene's protein. These numbers must somehow "normalized" on RNASeq to get more accurate results and the "Translation Efficiency" is aiming this. You can find more information on the Chothani et.al, deltaTE: Detection of Translationally Regulated Genes by Integrative Analysis of Ribo-seq and RNA-seq Data, 2019 (https://doi.org/10.1002/cpmb.108). <br />
You can see our workflow for the Translation Efficiency below: <br />
<br />

![TE_Diagram](Visuals/TE_Diagram.jpg)

## Results

Translation Efficiency analysis results are contains total 5 graphs and 2 data table along with count tables of each sample.

## Tables

### Translation Efficiency.tsv

geneID | baseMean | log2FoldChange | lfcSE | stat | pvalue | padj
--- | --- | --- | --- | --- | --- | ---
ENSG000000012345 | 400 | 0.22 | 0.10 | 2 | 0.05 | 0.1
ENSG000000067890 | 1500 | -1.2 | 0.11 | -9.4 | 1.8e-20 | 3.01e-19
ENSG000000001234 | 70 | 0.53 | 0.7 | 0.8 | 0.401 | 1
... | ... | ... | ... | ... | ... | ... 

This table contains results of Translation Efficiency analysis. P-adjusted values are calculated by using 'Benjamini-Hochberg' method. The details about the values in the table can be found in DESeq2 vignette (https://bioconductor.org/packages/release/bioc/vignettes/DESeq2/inst/doc/DESeq2.html)

### Differential Expression.tsv

geneID | baseMean.RiboSeq | log2FoldChange.RiboSeq | lfcSE.RiboSeq | stat.RiboSeq | pvalue.RiboSeq | padj.RiboSeq | baseMean.RNASeq | log2FoldChange.RNASeq | lfcSE.RNASeq | stat.RNASeq | pvalue.RNASeq | padj.RNASeq | Movement
--- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- 
ENSG000000012345 | 400 | 0.22 | 0.10 | 2 | 0.05 | 0.1 | 410 | -0.2 | 0.18 | -1.2 | 0.25 | 0.9 | Stable
ENSG000000067890 | 1500 | -1.2 | 0.11 | -9.4 | 1.8e-20 | 3.01e-19 | 8123 | 0.0005 | 0.2 | 0.07 | 0.9 | 0.99 | Translation
ENSG000000001234 | 70 | 0.53 | 0.7 | 0.8 | 0.401 | 1 | 77 | -1.2 | 0.8 | -1.5 | 0.16 | 1 | Transcription
... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... | ... 

The table contains Differential Expression results for both RiboSeq and RNASeq. The results related to RiboSeq can be found in the columns identified with ".RiboSeq" and the results related with RNASeq can be found in the columns identified with ".RNASeq". This table is merge of two different DE analysis and **IT IS NOT RELATED WITH TRANSLATION EFFICIENCY ANALYSIS**. These DE analyses shows regulation of the genes between Treatment vs. Control in each individual sequencing type. Since the values are not 'normalized' on sequencing type the log2 FoldChanges and p-values may or may not be different from TE. The logic of this analysis to see if the read counts in each gene are up-regulated or down-regulation in translation and transcription individually. By looking log2 FoldChanges in RNASeq and RiboSeq we are identifying genes with respect to table below;<br />
Description | Movement
--- | ---
log2FoldChannge RNASeq >= 1 and log2 FoldChange RiboSeq >= 1 | **"Homo-Direction"**
log2FoldChannge RNASeq <= -1 and log2 FoldChange RiboSeq <= -1 | **"Homo-Direction"**
log2FoldChannge RNASeq >= 1 and log2 FoldChange RiboSeq <= -1 | **"Cross-Direction"**
log2FoldChannge RNASeq <= -1 and log2 FoldChange RiboSeq >= 1 | **"Cross-Direction"**
log2FoldChannge RNASeq is between (-1,1) and log2 FoldChange RiboSeq >= 1 | **"Translation"**
log2FoldChannge RNASeq is between (-1,1) and log2 FoldChange RiboSeq <= -1 | **"Translation"**
log2FoldChannge RNASeq >= 1 and log2 FoldChange RiboSeq is between (-1,1) | **"Transcription"**
log2FoldChannge RNASeq <= -1 and log2 FoldChange RiboSeq is between (-1,1) | **"Transcription"**
log2FoldChannge RNASeq is between (-1,1) and log2 FoldChange RiboSeq is between (-1,1) | **"Stable"**

## Plots

## Prencipal Component Analysis (PCA) Plots

The results are contains 3 PCA plots "Sample", "RNASeq" and "RiboSeq". Sample is the results of PCA analysis of all given samples (RiboSeq + RNASeq for each Control and Treated) while RNASeq and RiboSeq contains results of RiboSeq and RNASeq samples respectively.


## Directional Plot

![directional](/Visuals/DirectionalPlot.jpg)

This is the visualized version of the differential expression data. Each dot represents a gene, X-Axis shows the log2FoldChanges in RiboSeq and Y-Axis shows log2FoldChanges in RNASeq and genes are labeled with respect to their movement.


