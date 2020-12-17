# sig_zscore_tool
Compute robust and conventional standardized scores

## Synopsis:
`sig_zscore_tool` [--ds DS] [--dim DIM] [--bkg_space 
BKG_SPACE] [--min_var MIN_VAR] [--var_adjustment VAR_ADJUSTMENT] [--estimate_prct 
ESTIMATE_PRCT] [--zscore_method ZSCORE_METHOD] [--use_gctx USE_GCTX]

## Positional Arguments:

## Optional Arguments:

`--ds` *DS*
: Path to input dataset

`--dim` *DIM*
: Choose whether to z-score along on the rows or columns. Default is row. Options 
are {row|column}

`--bkg_space` *BKG_SPACE*
: Filepath for GRP file listing a subset of column or row-ids of the input 
dataset that should be used to estimate the location and dispersion parameters. 
The ids are dependent on the '--dim' attribute, if dim is row, column-ids 
should be specified, otherwise row-ids should be used. Default is to use the 
all values in the row (or column) as the background.

`--min_var` *MIN_VAR*
: Minimum variation as estimated by MAD or standard deviation for robust and 
standard zscore methods respectively. Default is 0.1

`--var_adjustment` *VAR_ADJUSTMENT*
: Adjustment for low variance. Valid options are:
'estimate' - estimate the minimum variance from the data using:
EST_VAR = PRCTILE(SIGMA, est_prct), where SIGMA is a vector of variation (e.g. 
MAD, stdev) computed along dim for the entire dataset. The minimum variance is 
set to MAX(EST_VAR, MIN_VAR);
'fixed' - Default value. Use 'min_var' for minimum variance.
'none' - Assume min_var is zero
. Default is fixed. Options are {estimate|fixed|none}

`--estimate_prct` *ESTIMATE_PRCT*
: Percentile to consider when estimating the minimum variance from the data. Only 
used when '--var_adjustment' is set set to 'estimate'. Default is 1

`--zscore_method` *ZSCORE_METHOD*
: Determines the type of z-scoring is performed.
'robust' -  uses median and median absolute deviation (MAD) to calculate 
z-scores as (X-NANMEDIAN(X))./(MAD(X)*1.4826)
'standard' - uses the mean and std to calculate z-scores as (X-NANMEAN(X))./ 
NANSTD(X). Default is robust. Options are {robust|standard}

`--use_gctx` *USE_GCTX*
: Save file in binary GCTX format if 1 else save as a text GCT. Default is 1. 
Options are {1,0}

## Description:
 The sig_zscore_tool returns a z-scored version on the input matrix X, the same 
dimensions as X. Two methods for computing Z-scores are currently supported. 
The default is to compute a robust z-score as: 
(X-NANMEDIAN(X))./(MAD(X)*1.4826). Alternatively a standard zscore can be 
specified and is computed as: (X-NANMEAN(X))./ NANSTD(X). In addition 
adjustments are made to account for low variance based on heuristics controlled 
via the var_adjustment parameter.
 
## Examples:
 
% Run row-wise z-scoring using the entire dataset
 
sig_zscore_tool('--ds', 'raw_data.gctx');
 
% Run column-wise z-scoring using a custom background space
 
sig_zscore_tool('--ds', 'raw_data.gctx', '--dim', 'column', '--bkg_space', 
'rowids.grp');
 
 

