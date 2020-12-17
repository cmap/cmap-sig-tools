# sig_getgenesets_tool
Extract top and botton N genes from dataset

## Synopsis:
`sig_getgenesets_tool` [--ds DS] [--set_size 
SET_SIZE] [--cid CID] [--rid RID] [--row_space ROW_SPACE] [--sort_order SORT_ORDER] 
[--es_tail ES_TAIL] [--id_field ID_FIELD] [--desc_field DESC_FIELD] [--suffix SUFFIX] 
[--dim DIM] [--enforce_set_size ENFORCE_SET_SIZE] [--min_set_size MIN_SET_SIZE]

## Arguments

`--ds` *DS*
: Input dataset

`--set_size` *SET_SIZE*
: Integer, size of each geneset. Default is 50

`--cid` *CID*
: List of column ids to extract as a GRP file or cell array.

`--rid` *RID*
: List of row ids to extract as a GRP file or cell array

`--row_space` *ROW_SPACE*
: Common row-id spaces to extract. Use '_probeset' if affy_ids present.. Default 
is custom. Options are {lm|lm_probeset|bing|bing_probeset|full|custom}

`--sort_order` *SORT_ORDER*
: String, sort order of gene expression. Default is descend. Options are 
{descend|ascend}

`--es_tail` *ES_TAIL*
: String, specifies which tail to return.. Default is both. Options are 
{both|up|down}

`--id_field` *ID_FIELD*
: String, specifies an alternate metadata field to use for selecting features 
instead of ids.. Default is _id

`--desc_field` *DESC_FIELD*
: String, specifies a metadata field to use for the output desc field.

`--suffix` *SUFFIX*
: String, Append string to each set name.

`--dim` *DIM*
: String, Dimension of matrix to operate on.. Default is column. Options are 
{column|row}

`--enforce_set_size` *ENFORCE_SET_SIZE*
: Boolean, Assert if set sizes match N exactly. A set size of less than N can 
result in cases where an alternate id_field is specified and there are 
duplicate entries. Set to false to disable this constraint.. Default is 1

`--min_set_size` *MIN_SET_SIZE*
: Integer, minimum set size. Sets with fewer members are excluded from the 
ouput.. Default is 0

## Description:
 
Returns the top and bottom N genesets for the score dataset DS sorted according 
to --sort_order. SORTORDER can be 'descend' or 'ascend'. UP and DN are 
structures with length equal to the number of columns in DS. Supports 
subsetting to specified row_space before extracting genesets with --row_space 
option.
 
Outputs a GMT file.
 
Each row in the structure has the the following fields: hd : String header, 
same as the column id in DS with either '_UP' or '_DN' appended. desc :  String 
descriptor. Set to ''. entry : Cell array of N row identifiers in DS.
 
## Examples:
 
% Get top and bottom 50 genes of each column in given dataset
 
sig_getgenesets_tool('--ds', 'raw_data.gctx', '--set_size', 50);
 
% Get top and bottom 50 landmark genes of each column in given gene id dataset
 
sig_getgenesets_tool('--ds', 'raw_data.gctx', '--set_size', 50, '--row_space', 
'lm');
 
% Get top and bottom 50 landmark genes of each column in given affy id dataset
 
sig_getgenesets_tool('--ds', 'raw_data.gctx', '--set_size', 50, '--row_space', 
'lm_probeset');
 
 

