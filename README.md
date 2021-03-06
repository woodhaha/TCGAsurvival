# Scripts to extract TCGA data for survival analysis.

- `CELLX_analysis.Rmd` - Tumor-normal expression of selected gene in all TCGA cancers. In which cancers expression of the selected gene up- or downregulated the most in tumor vs. normal comparison.

- `survival.Rmd` - a pipeline to run survival analyses. Based on `survival.R`
    - `Analysis 1` - Selected genes, selected cancers, no clinical annotations. Results are in `res.genes.Analysis1` folder.
    - `Exploratory` - All genes, selected cancers, no clinical annotations. Not run by default.
    - `Analysis 2` - Selected genes, all (or selected) cancers, no clinical annotations. Results are in `res.genes.Analysis2` folder.
    - `Analysis 3` - Selected genes, all (or, selected) cancers, all unique clinical (sub)groups. Results are in `res.genes.Analysis3` folder. Open file `global_stats.txt` in Excel, sort by p-value (Cox proportional hazard analysis) and explore in which clinical (sub)groups expression of the selected gene affects survival the most.
    - `Analysis 4` - Selected genes, selected cancers, all combinations of clinical annotations. Not run by default.

- `TCGA_summary.Rmd` - in which cancers, and clinical subgroups, expression of the selected gene affects survival the most. Uses results from `res.genes.Analysis2` and `res.genes.Analysis3` folders.

- `correlation.Rmd` - Co-expression analysis of selected gene vs. all others, in selected cancers. Genes best correlating with the selected gene may share common functions, described in the KEGG canonical pathway analysis section.

- `TCGA_DEGs.Rmd` - differential expression analysis of TCGA cohorts separated into groups with high/low expression of selected genes. The results are similar to the `correlation` results, most of the differentially expressed genes are also best correlated with the selected genes. This analysis is to explicitly look at the extremes of the selected gene expression and identify KEGG pathways that may be affected.


- `TCGA2SynTarget.R` - prepares TCGA data for SynTarget format. Now, general data processing script. Start there to load cancer-specific expression and clinical data using `load_data` and `summarize_data` functions.

- `Supplemental_R_script_1.R` - a modified script to run gene-specific or global survival analysis, from [http://kmplot.com](http://kmplot.com), [Source](http://kmplot.com/analysis/studies/Supplemental%20R%20script%201.R)

# TCGA data

Public data is available through the [TCGA2STAT R package](http://www.liuzlab.org/TCGA2STAT/).

- [Cancer types](http://www.liuzlab.org/TCGA2STAT/CancerDataChecklist.pdf)
- [Data types](http://www.liuzlab.org/TCGA2STAT/DataPlatforms.pdf)
- [Clinical values](http://www.liuzlab.org/TCGA2STAT/ClinicalVariables.pdf)


# [SynTarget](http://www.chemoprofiling.org/cgi-bin/GEO/cancertarget/web_run_CT.V0.S1.pl) tool

SynTarget data format:

1. expression matrix - text file:
 
sample;SampleId1;SampleId2;SampleId3;...
probeID1;37.7;45,5;67.54;...
probeID2;37.7;45,5;67.54;...

2. mapping probes to genes
 
probeID1;geneID1   (preferably official gene symbols or NCBI ENTREZ IDs)
probeID2;geneID2
..

3. Sample Annotation (survival time + clinical variables):
 
!! The fisrt 3 column names are mandatory, should be "sample_id, surv_time, dead_1_alive_0". The others are arbitrary (if available), and should specify clinical variables names (i.e. "stage", "P53_mutation_status")
 
sample_id, surv_time, dead_1_alive_0, stage, P53_mutation_status, cellularity, lymph_nodes_positive, ...
MB-0101,90.4,0, 2, WT, moderate, 0, HT/RT, 2,..
MB-4832,141.7,1, 0, WT, moderate, 0, RT, 1, ..
MB-5119,59.8,0, 0, NA, moderate, 0, NONE, 2,..
MB-0117,8.2,0, 2, WT, moderate, 1, HT/RT, 2, ..
