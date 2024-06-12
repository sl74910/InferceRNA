# Infercerna Usage Guide

## Introduction

This R package includes functions for scoring lncRNA-miRNA-mRNA interactions and calculating PMI (Pointwise Mutual Information) and correlation. The `calculatePMICorrDF` function is used to score lncRNA-miRNA-mRNA interactions based on correlation and information measures. It takes various expression data as input and calculates the Pearson correlation coefficient, pointwise mutual information (PMI), and information interaction (II) measure. The resulting scores are sorted based on the absolute difference in PMI and organized into a dataframe.

The `calcPMIAndCorr` function calculates the PMI and correlation between a given lncRNA, miRNA, and mRNA. It requires the names of the lncRNA, miRNA, and mRNA, along with their expression data in tumor and normal tissues. It also requires the differential expression analysis results for the lncRNA and mRNA. The returned vector contains key information about the interactions between lncRNA, miRNA, and mRNA, including their names, fold change values from differential expression analysis, Pearson correlation coefficient, as well as PMI and information interaction measure computed based on expression data.

These functions can assist researchers in assessing and understanding the associations and interactions among lncRNA, miRNA, and mRNA, providing support for further functional and mechanistic studies.

## `checkceRNA_inputs()`

This function checks the input data for ceRNA (competing endogenous RNA) analysis, including lncRNA expression data, miRNA expression data, and mRNA expression data for both tumor and normal tissue. By default, it filters out the rows in the dataset that have zero expression values across at least half of the samples.

```r
library(InferceRNA)
data("PTENceRNA")

inferCERNA <- checkceRNA_inputs(
    lncRNAexpression = PTENceRNA$ceRNA1,
    lncRNAexpression_tumor = PTENceRNA$ceRNA1_tumor,
    lncRNAexpression_normal = PTENceRNA$ceRNA1_normal,
    miRNAexpression = PTENceRNA$miRNA,
    miRNAexpression_tumor = PTENceRNA$miRNA_tumor,
    miRNAexpression_normal = PTENceRNA$miRNA_normal,
    mRNAexpression = PTENceRNA$ceRNA2,
    mRNAexpression_tumor = PTENceRNA$ceRNA2_tumor,
    mRNAexpression_normal = PTENceRNA$ceRNA2_normal,
    threshold = 0.5
)
## `calcPMIAndCorr()`

Calculating PMI and correlation. This function calculates the Pointwise Mutual Information (PMI) and correlation between a lncRNA, miRNA, and mRNA based on their expression data and differential expression analysis results. The function takes as input the names of the lncRNA, miRNA, and mRNA, along with their expression data in tumor and normal tissues. The returned vector contains key information about the interactions between lncRNA, miRNA, and mRNA, including their names, fold change values from differential expression analysis, Pearson correlation coefficient, as well as PMI and information interaction measure computed based on expression data.

```r
library(InferceRNA)
data("PTENceRNA")

# The function depends on calculatePMICorrDF1 and calculatePMICorrDF2 for optimizing the Windows and Linux systems respectively.

result <- calcPMIAndCorr(
    lncRNA = "PTENP1",
    miRNA = "hsa-miR-582-3p",
    mRNA = "PTEN",
    inferCERNA = PTENceRNA,
    DElncRNA, 
    DEmRNA
)

# colnames = (lncRNA, miRNA, mRNA,
#             DElncRNA$log2FoldChange[DElncRNAindex],
#             DEmRNA$log2FoldChange[DEmRNAindex], 
#             PearsonCC, PMI.normal, PMI.tumor, absdiffPMI, diffCMIMI)
# [1] "PTENP1"             "hsa-let-7a-5p"      "PTEN"               
# [4] "-0.508686920560346" "-0.439408139255671" "0.781228077121531"  
# [7] "0.674431171109052"  "0.920363630693052"  "0.245932459584"     
# [10] "0.353907540952865"
