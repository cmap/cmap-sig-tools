# SigSliceTool
Extract a subset from a larger dataset

## Synopsis:
`SigSliceTool` [--ds DS] [--cid CID] [--rid RID] 
[--row_space ROW_SPACE] [--row_meta ROW_META] [--col_meta COL_META] [--use_gctx USE_GCTX] 
[--num_digits NUM_DIGITS] [--ignore_missing IGNORE_MISSING]

## Positional Arguments:

## Optional Arguments:

`--ds` *DS*
: Dataset in GCT or GCTX format

`--cid` *CID*
: List of column ids to extract as a GRP file or cell array.

`--rid` *RID*
: List of row ids to extract as a GRP file or cell array

`--row_space` *ROW_SPACE*
: Common row-id spaces to extract. '_probeset' refers to affy ids. Default is 
custom. Options are 
{lm|lm_probeset|bing|bing_probeset|aig|full_probeset|custom}

`--row_meta` *ROW_META*
: Row metadata as a TSV text file. If provided the rows in the output dataset 
will be annotated using the first field as the key to join with the row-ids

`--col_meta` *COL_META*
: Column metadata as a TSV text file. If provided the columns in the output 
dataset will be annotated using the first field as the key to join with the 
column-ids

`--use_gctx` *USE_GCTX*
: Save results in GCTX format if true or GCT otherwise. Default is 1

`--num_digits` *NUM_DIGITS*
: Number of digits to use when writing to GCT format. Default is 4

`--ignore_missing` *IGNORE_MISSING*
: If false, program will fail when missing any specified rids or cids. Default is 
0

## Description:
This tool extracts a subset of rows and columns from a larger dataset.
 
## Examples:
 
% Extract a subset of columns from a larger dataset
 
SigSliceTool('--ds', 'data.gctx', '--cid', 'column_ids.grp', '--rid', 
'row_ids.grp')
 
% Extract only landmark genes for a subset of columns
 
SigSliceTool('--ds', 'data.gctx', '--cid', 'columns.grp', '--row_space', 'lm')
 
 

