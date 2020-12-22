# sig_gutc_tool
Compute similarity of input queries to CMap perturbagens, adjusting the results w.r.t to a background distribution

## Synopsis
`sig_gutc_tool` [--query_result QUERY_RESULT] [--up, 
--uptag UP] [--down, --dntag DOWN] [--query_meta QUERY_META] [--is_matched IS_MATCHED] 
[--match_group MATCH_GROUP] [--metric METRIC] [--es_tail ES_TAIL] [--score SCORE] [--rank 
RANK] [--build_id BUILD_ID] [--feature_space FEATURE_SPACE] [--sample_space SAMPLE_SPACE] 
[--pcl_set PCL_SET] [--bkg_path BKG_PATH] [--save_matrices SAVE_MATRICES] [--save_digests 
SAVE_DIGESTS]

## Arguments

`--query_result` *QUERY_RESULT*
: Load pre-computed query results from supplied connectivity matrix.

`--up, --uptag` *UP*
: Geneset(s) to use for the up portion of the query

`--down, --dntag` *DOWN*
: Geneset(s) to use for the down portion of the query

`--query_meta` *QUERY_META*
: Metadata for each query. This is required for matched_mode. The following 
fields are required for matching with default parameters: [pert_id, cell_id, 
pert_idose, pert_itime]

`--is_matched` *IS_MATCHED*
: If true, compute GUTC in cell-line matched mode. Default is 0

`--match_group` *MATCH_GROUP*
: Query grouping variable(s) for cell-line matching. Note that the tool expects 1 
query per cell-line for each unique grouping. Default is 
pert_id|pert_idose|pert_itime

`--metric` *METRIC*
: Similarity metric. Default is wtcs. Options are {wtcs}

`--es_tail` *ES_TAIL*
: Specify two-tailed or one-tailed statistic for enrichment metrics. Default is 
both. Options are {both|up|down}

`--score` *SCORE*
: Custom dataset of differential expression scores (e.g. zscores) in GCT(X) 
format. Use in combination with rank parameter.

`--rank` *RANK*
: Custom dataset of ranks corresponding to the score matrix in GCT(X) format. Use 
in combination with score parameter. Note that if

`--build_id` *BUILD_ID*
: Data build identifier. a2 refers to the GSE92742 dataset with Affymetrix 
feature ids. a2geneid is the same dataset mapped to Entrez GeneIDs. Default is 
a2geneid. Options are {a2|a2geneid}

`--feature_space` *FEATURE_SPACE*
: Feature space for query comparisions. Select lm for landmark space, bing for 
best-inferred gene space or full for complete genespace. Default is bing. 
Options are {lm|bing|full}

`--sample_space` *SAMPLE_SPACE*
: Signature space. Default is full. Options are {full}

`--pcl_set` *PCL_SET*
: Perturbational classes in GMT format. Default is 
/cmap/data/vdb/touchstone_v1.1/matched/annot/pcl_n171_20170201.gmt

`--bkg_path` *BKG_PATH*
: Path to background signature definition and percentile transforms. Default is 
/cmap/data/vdb/touchstone_v1.1/matched

`--save_matrices` *SAVE_MATRICES*
: Save result matrices. Default is 1

`--save_digests` *SAVE_DIGESTS*
: Save per-query digest folders. Default is 1

## Description
Sig GUTC computes the similarity between input genesets (queries) and 
perturbational gene expression signatures in the CMap database. The results are 
transformed to a percentile scale and reported at different levels of 
granularity to aid interpretation.
 
Briefly the algorithm operates as follows. First raw similarity scores between 
a query and CMap signatures are computed. While the method is agnostic to the 
specific similarity metric used, the default choice is a two-tailed weighted 
enrichment score.
 
The raw scores are then scaled (Normalized) to adjust for co-variates like cell 
line and the type of perturbation. The normalized scores are transformed to 
percentile scores by comparing the test scores to those of a reference 
collection of signatures called Touchstone.
 
The per-signature normalized connectivity scores are summarized to yield 
connectivity to individual perturbagens within a cell line, across-cell lines 
and for perturbational classes (PCLs). Any summary statistic can be employed, 
but in practice the maximal-quantile (MAXQ) score is used. Given a set of 
scores X and a pair of percentiles PL and PU, MAXQ returns the percentile value 
of X that has the maximum absolute value (By default GUTC uses PL=33 and 
PU=67).
 
At each level of summarization, percentile scores are re-computed by comparing 
to the corresponding results when applied to the Touchstone signatures. For a 
given connection, the percentiles are computed within perturbagens with the 
cell type that the connection corresponds to.
 
An important variant of GUTC is the matched mode specified by the is_matched 
parameter. Matched mode incorporates cell-line information when query data has 
been generated systematically in cell types that match the touchstone 
signatures. Currently this includes the following 9 cell types : [A375, A549, 
HEPG2, HCC515, HA1E, HT29, MCF7, PC3, VCAP]. To run GUTC in this mode, the 
is_matched flag should be set to true. Also, the required metadata should be 
provided using the query_meta argument. Note that the the tool expects 1 query 
per cell-line for each unique [pert_id, pert_idose, pert_itime] combination. 
The default query grouping variables can be changed using the match_group 
argument.
 
## Examples
 
- Run queries and apply GUTC
 
`sig_gutc_tool --up 'up.gmt' --down 'down.gmt'`
 
- Apply GUTC on pre-computed query results
 
`sig_gutc_tool --query_result '/path/to/sig_query/results/wtcs.gctx'`
 
- Run GUTC in cell-line matched mode
 
`sig_gutc_tool --query_result '/path/to/sig_query/results/wtcs.gctx' --query_meta '/path/to/query_info.txt' --is_matched true`
 
- Run GUTC using a custom dataset, Expects that 

`sig_gutc_tool --bkg_path '/path/to/gutc_background' --score '/path/to/modzs.gctx' --rank '/path/to/rank.gctx' --up 'up.gmt' --down 'down.gmt'`
 
 

