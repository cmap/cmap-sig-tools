# sig_recall_tool
Compare replicates signatures to assess similarity

## Synopsis:
`sig_recall_tool` [--ds_list DS_LIST] [--metric 
METRIC] [--set_size SET_SIZE] [--es_tail ES_TAIL] [--dim DIM] [--sample_field 
SAMPLE_FIELD] [--feature_field FEATURE_FIELD] [--row_filter ROW_FILTER] [--column_filter 
COLUMN_FILTER] [--save_pw_matrix SAVE_PW_MATRIX] [--recall_group_prefix 
RECALL_GROUP_PREFIX] [--outlier_alpha OUTLIER_ALPHA] [--fix_ties FIX_TIES]

## Arguments

`--ds_list` *DS_LIST*
: List of datasets to compare.
A single replicate set can be specified as a GRP file or cell array listing the 
full filepath to a dataset per line. Multiple replicate sets can be specified by supplying a TSV text file with the 
following columns:

`group_id`: *Grouping variable shared by all datasets in a replicate set*

`file_path`: *Full filepath to a dataset*

For example to run recall on two replicate sets A and B use:

|group_id |   file_path|
|---|---|
|A|/path/to/DS_A_X1.gct|
|A|/path/to/DS_A_X2.gctx|
|A| /path/to/DS_A_X3.gct|
|B|/path/to/DS_B_X1.gctx|
|B|/path/to/DS_B_X2.gct|

Any replicate set with singleton entries will be ignored. A list of skipped 
replicate sets is output to a file named ds_skipped.grp 

`--metric` *METRIC*
: Similarity metric to use for the comparison. Default is spearman. Options are 
{spearman|pearson|wtcs|cs|cosine}

`--set_size` *SET_SIZE*
: Set size to use for enrichment metrics. This is ignored for correlation 
metrics. Default is 50

`--es_tail` *ES_TAIL*
: Specify two-tailed or one-tailed statistic for enrichment metrics. Default is 
both. Options are {up|down|both}

`--dim` *DIM*
: Dimension to operate on. The default is to compare columns between datasets. If 
'row' is specified, the features are compared. Default is column. Options are 
{row|column}

`--sample_field` *SAMPLE_FIELD*
: Column metadata field to use for matching pairs of comparisons. The field 
should exist in each dataset for the dimension specified. Default is det_well

`--feature_field` *FEATURE_FIELD*
: Row metadata field to use for matching pairs of comparisons. The field should 
exist in each dataset for the dimension specified. Default is rid

`--row_filter` *ROW_FILTER*
: GMT or GMX file specifying row filter criteria. Dataset rows are filtered prior 
to recall analysis. See parse_filter for details on the filter format

`--column_filter` *COLUMN_FILTER*
: GMT or GMX file specifying column filter criteria. Dataset columns are filtered 
prior to recall analysis. See parse_filter for details on the filter format

`--save_pw_matrix` *SAVE_PW_MATRIX*
: If true, saves pairwise similarity matrices in GCTx format for each pair of 
comparisons.. Default is 0

`--recall_group_prefix` *RECALL_GROUP_PREFIX*
: String if provided is prepended to the recall_group field in the recall report

`--outlier_alpha` *OUTLIER_ALPHA*
: Level of significance, used to flag outlier replicate datasets. Default is 0.01

`--fix_ties` *FIX_TIES*
: Adjusts for ties in the recall score when computing ranks if true. Default is 1

## Description
For a grouped collection of datasets (a dataset group), the recall tool 
computes pairwise similarities between each pair of datasets in the group using 
the specified metric and dimension. In the case of non-symmetric enrichment 
metrics (e.g. wtcs) the similarity is assessed in both directions and averaged 
to ensure that the order of evaluation of matrices does not affect the result. 
Note that if the dimensions of the input datasets are of different sizes the 
pairwise similarity matrix will not be square. The recall scores are elements 
of the pairwise similarity matrix that correspond to matching sample_field 
metadata values (or feature_field if the dimension is row). Next row and column 
ranks are computed by ranking elements of similarity matrix both row and 
column-wise and converted to percentiles (ranging [0, 100] with 0 indicating 
perfect recall). The recall rank is computed as the average of the row and 
column percentile ranks for the same elements that correspond to the recall 
scores.
 
The tool produces several outputs, including a summary HTML index page that 
lists the recall summary for each dataset group in the input. The table links 
to a gallery of diagnostic plots for each group. In addition the following TSV 
text reports are generated for each dataset group:
 
1. recall_report_pairs.txt : This report provides the most granular level 
information of the analysis and lists the recall scores and ranks of every pair 
of profiles compared in addition to the corresponding metadata. The key recall 
fields are:

    - `recall_group` : indicates the pairwise comparisions belonging to the same 
    replicate set
    - `recall_score` : similarity score of the pair of signatures. 
    - `recall_rank` : The average percentile rank computed from the row-wise and 
    column-wise percentile ranks of the underlying pairwise similarity matrix.
    - `recall_composite` : A combined measure ranging [0, 1] derived from the recall 
    score and rank. Its computed as the geometric mean of clipped recall_score and 
    recall rank as follows:
    `recall_composite = sqrt(clip(recall_score, 0.001, inf).* (100 - 
    recall_rank)/100)`
 
2. recall_report_sets.txt : A replicate set level report listing aggregate 
statistics for each unique recall_group derived from the metrics listed above.
 
3. recall_summary.txt : A summary of recall of all replicate sets in a dataset 
group. In addition lists the presence and identity of outlier datasets.
 
4. recall_report_datasets.txt : Recall statistics for each dataset belonging to 
a dataset group. In addition the recall ranks associated with each dataset are 
compared with each other for outliers.
 
## Examples
 
- Compute recall using Spearman correlation
 
`sig_recall_tool --ds_list '/list/of/datasets' --metric 'spearman'` 

- Compute recall using Two-tailed weighted Enrichment with a set size of 50
 
`sig_recall_tool --ds_list '/list/of/datasets' --metric 'wtcs' --set_size 50`
 
 

