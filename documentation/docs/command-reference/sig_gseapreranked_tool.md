# sig_gseapreranked_tool
Run pre-ranked GSEA on a ranked matrix

## Synopsis
`sig_gseapreranked_tool` [--up, --uptag UP] 
[--score SCORE] [--rank RANK] [--sig_meta SIG_META] [--metric METRIC] [--es_tail ES_TAIL] 
[--num_perm NUM_PERM] [--query_meta QUERY_META] [--save_minimal SAVE_MINIMAL] 
[--save_digests SAVE_DIGESTS] [--max_col MAX_COL]

## Arguments

`--up, --uptag` *UP*
: Geneset(s) to use for the up portion of the query

`--score` *SCORE*
: Custom dataset of differential expression scores (e.g. zscores) in GCT(X) 
format. Use in combination with rank parameter.

`--rank` *RANK*
: Custom dataset of ranks corresponding to the score matrix in GCT(X) format. Use 
in combination with score parameter.

`--sig_meta` *SIG_META*
: Signature metadata for each column in the score matrix. This is a required 
field. The first field must match the column id field in the score matrix.

`--metric` *METRIC*
: Similarity metric. Default is wtcs. Options are {wtcs|cs}

`--es_tail` *ES_TAIL*
: Specify direction of one-tailed statistic for enrichment metrics. Default is 
up. Options are {up|down}

`--num_perm` *NUM_PERM*
: Number of null permutations per unique set size. Default is 500

`--query_meta` *QUERY_META*
: Metadata for each query.

`--save_minimal` *SAVE_MINIMAL*
: Save minimal output to optimize storage requirements. For enrichment based 
metrics only the combined scores are saved. Default is 1

`--save_digests` *SAVE_DIGESTS*
: Save per-query digests. Default is 1

`--max_col` *MAX_COL*
: Maximum number of columns of the score/rank matrices to read at a time. Default 
is 25000

## Description
The tool computes set-based enrichment analysis against a user-defined 
rank-ordered dataset.  It determines whether a priori defined sets show 
statistically significant enrichment at either end of the ranking.
 
## Examples
 
- Run preRanked GSEA
 
`sig_gseapreranked_tool --up 'up.gmt' --score 'score.gctx'`
 

