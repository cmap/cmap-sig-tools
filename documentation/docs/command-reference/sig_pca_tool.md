# sig_pca_tool
Perform Principal Components Analysis on a dataset

## Synopsis
`sig_pca_tool` [--ds DS] [--ds_meta DS_META] 
[--sample_dim SAMPLE_DIM] [--cid CID] [--rid RID] [--row_space ROW_SPACE] 
[--disable_table DISABLE_TABLE]

## Arguments

`--ds` *DS*
: Input dataset

`--ds_meta` *DS_META*
: Optional annotations as a TSV table for the input dataset for the dimension 
being operated on. The first column must match the corresponding id field in ds

`--sample_dim` *SAMPLE_DIM*
: Dimension of the dataset corresponding to samples or observations. Default is 
column. Options are {1|2|column|row}

`--cid` *CID*
: List of column ids to use specified as a GRP file or cell array. If empty all 
columns are used.

`--rid` *RID*
: List of row ids to to use specified as a GRP file or cell array. If empty all 
rows are used

`--row_space` *ROW_SPACE*
: Common row-id space definitions to use as an alternative to the rid parameter. 
Default is all. Options are 
{all|lm|bing|aig|lm_probeset|bing_probeset|full_probeset|custom}

`--disable_table` *DISABLE_TABLE*
: Disable generating annotated text table for first two components. The table can 
be generated post-hoc from the saved pc_score matrix if needed.. Default is 0

## Description
This tool applies Principal Components Analysis (PCA) on raw data.
 
## Examples
 
- Apply PCA on columns of a matrix
 
`sig_pca_tool --ds 'raw_data.gctx'`
 
- Merge datasets from a list of folders
 
`sig_pca_tool --folders 'folders.grp' --cid 'columns.grp' --row_space 'lm'`
 
 

