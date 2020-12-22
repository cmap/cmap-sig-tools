# sig_collate_tool
Merge datasets

## Synopsis
`sig_collate_tool` [--files FILES] 
[--folders FOLDERS] [--file_wildcard FILE_WILDCARD] [--parent_folder PARENT_FOLDER] 
[--sub_folder SUB_FOLDER] [--row_space ROW_SPACE] [--rid RID] [--cid CID] [--exclude_rid 
EXCLUDE_RID] [--exclude_cid EXCLUDE_CID] [--use_gctx USE_GCTX] [--use_compression 
USE_COMPRESSION] [--block_size BLOCK_SIZE] [--merge_partial MERGE_PARTIAL] 
[--missing_value MISSING_VALUE]

## Arguments

`--help, -h`
: Show this help message and exit

`--help_markdown`
: Print help in Markdown format

`--undef_action` *UNDEF_ACTION*
: Action to take if an undefined argument is encountered. Default is error. 
Options are {error|warn|ignore}

`-o, --out` *OUT*
: Output path

`--runtests`
: Execute functional and unit tests. Default is 0

`--rundemo`
: Run the tool with sample inputs. Default is 0

`--rpt` *RPT*
: Report folder prefix

`--create_subdir` *CREATE_SUBDIR*
: Create subfolder within out for saving results. Default is 1

`--verbose` *VERBOSE*
: Print debugging messages. Default is 1

`--encode_url` *ENCODE_URL*
: Encode all input URLs. Default is 0

`--config` *CONFIG*
: Argument configuration file

`--files` *FILES*
: List of files as a GRP file or cell array

`--folders` *FOLDERS*
: List of parent folders as a GRP file or cell array.

`--file_wildcard` *FILE_WILDCARD*
: Wildcard

`--parent_folder` *PARENT_FOLDER*
: Parent folder containing files or folders

`--sub_folder` *SUB_FOLDER*
: Sub folder, relative to the parent folder that contains the target file

`--row_space` *ROW_SPACE*
: Filter features or rows to a pre-defined space. Options are 
{|lm|bing|full|custom}

`--rid` *RID*
: List of row ids to include as GRP file or cell array. The list of ids are 
excluded if exclude_rid is true

`--cid` *CID*
: List of column ids to include as GRP file or cell array. The list of ids are 
excluded if exclude_cid is true

`--exclude_rid` *EXCLUDE_RID*
: Exclude features or rows specified by rid or row_space if true. Default is 0

`--exclude_cid` *EXCLUDE_CID*
: Exclude columns specified by cid or column_space if true. Default is 0

`--use_gctx` *USE_GCTX*
: Save results in GCTX format if true or GCT otherwise. Default is 1

`--use_compression` *USE_COMPRESSION*
: Use compression when saving in GCTX format. Default is 1

`--block_size` *BLOCK_SIZE*
: Number of files to read before writing output to disk. Default is 25

`--merge_partial` *MERGE_PARTIAL*
: Merge datasets with partially overlaping ids. Default is 0. Options are {1,0}

`--missing_value` *MISSING_VALUE*
: Number of files to read before writing output to disk. Default is nan

## Description
This tool merges a list of datasets.
 
## Examples
 
- Merge a list of files
 
`sig_collate_tool --files 'dslist.grp' --row_space lm`
 
- Merge datasets from a list of folders
 
`sig_collate_tool --folders 'folders.grp' --cid 'columns.grp' --row_space 'lm'`
 
- Merge files names score_n*.gctx from a subfolder zs/ within a list of folders
 
`sig_collate_tool --folders 'folders.grp' --file_wildcard 'score_n*.gctx' --sub_path 'zs/'`
 
 

