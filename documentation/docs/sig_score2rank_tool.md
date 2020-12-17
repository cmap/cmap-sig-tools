# sig_score2rank_tool
Generate rank matrix from score matrix

## Synopsis:
`sig_score2rank_tool` [--ds DS] [--outfile 
OUTFILE] [--dim DIM] [--sort_order SORT_ORDER] [--ignore_nan IGNORE_NAN] [--as_fraction 
AS_FRACTION] [--as_percentile AS_PERCENTILE] [--fix_ties FIX_TIES] [--use_gctx USE_GCTX] 
[--read_mode READ_MODE] [--block_size BLOCK_SIZE]

## Arguments:

`--ds` *DS*
: Input score matrix to compute ranks

`--outfile` *OUTFILE*
: Output filename of rank matrix.

`--dim` *DIM*
: Dimension to operate on.. Default is column. Options are {column|row}

`--sort_order` *SORT_ORDER*
: Sort order. Default is descend. Options are {descend|ascend}

`--ignore_nan` *IGNORE_NAN*
: Ignore NaNs when ranking. Note that ignoring NaNs is slower. Default is 1

`--as_fraction` *AS_FRACTION*
: Logical, returns ranks as a fraction of total rows. Default is 0

`--as_percentile` *AS_PERCENTILE*
: Logical, returns ranks as a percentile of total rows. Default is 0

`--fix_ties` *FIX_TIES*
: Logical, adjusts for ties (is false by default).. Default is 0

`--use_gctx` *USE_GCTX*
: Use GCTX file format. Default is 1

`--read_mode` *READ_MODE*
: Specify how to process the input matrix. If full is specified the complete 
matrix is processed enmasse. If iterative the input matrix is processed in 
blocks. Default is full. Options are {full|iterative}

`--block_size` *BLOCK_SIZE*
: size of blocks to process if the read_mode is iterative. Default is 50000

## Description:
 
 
## Examples:
 
% Calculate ranks of input score matrix
 
sig_score2rank_tool('--ds', 'raw_data.gctx');
 
 

