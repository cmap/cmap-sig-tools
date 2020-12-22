# sig_annotate_tool
Read or modify metadata for a dataset

## Synopsis
`sig_annotate_tool` [--ds DS] [--row_meta 
ROW_META] [--column_meta COLUMN_META] [--strip_matrix STRIP_MATRIX]

## Arguments

`--ds` *DS*
: Dataset in GCT or GCTX format

`--row_meta` *ROW_META*
: Row annotations as a TSV file. The first column or a column named id should 
match the row ids in the dataset

`--column_meta` *COLUMN_META*
: Column annotations as a TSV file. The first column or a column named id should 
match the column ids in the dataset

`--strip_matrix` *STRIP_MATRIX*
: If selected will output the original matrix with the specified annotations 
removed.. Default is none. Options are {row|column|both|none}

## Description
This tool extracts annotations of rows and columns from a dataset.
 
## Example
- Extract row and column annotations from a dataset
 
`sig_annotate_tool --ds 'data.gctx'`
 
- Update row and column annotations
 
`sig_annotate_tool --ds 'data.gctx' --row_meta 'row_meta.tsv' --column_meta 'column_meta.tsv'`
 
- Remove matrix column annotations
 
`sig_annotate_tool --ds 'data.gctx' --strip_matrix 'column'`
 
 

