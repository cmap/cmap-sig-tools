# sig_marker_tool
Identify differentially expressed genes using two-class marker selection.

## Synopsis
`sig_marker_tool` [--ds DS] [--col_meta COL_META] 
[--row_meta ROW_META] [--phenotype PHENOTYPE] [--metric METRIC] [--feature_space 
FEATURE_SPACE] [--feature_id FEATURE_ID] [--ignore_missing_features 
IGNORE_MISSING_FEATURES] [--islog2 ISLOG2] [--nmarker NMARKER] [--fix_low_var 
FIX_LOW_VAR] [--min_sample_size MIN_SAMPLE_SIZE] [--use_gctx USE_GCTX] [--skip_rpt 
SKIP_RPT] [--add_heatmap_fields ADD_HEATMAP_FIELDS]

## Arguments

`--ds` *DS*
: Dataset of gene expression profiles [GCT or GCTX]

`--col_meta` *COL_META*
: Optional column metadata table

`--row_meta` *ROW_META*
: Optional row metadata table

`--phenotype` *PHENOTYPE*
: Phenotype class definition file [TSV]. It should include sample_id, and 
class_id and sig_id fields and optionally class_label. The sample_id field 
corresponds to column identifiers in the supplied dataset. The following 
convention is preferred class_id = A denotes the treatment class or class of 
interest and class_id = B is the control class. The sig_id field specifies the 
name of the output signature. Here is an example:

|sample_id|	class_id|	class_label|	sig_id|
|---|---|---|---|
| 	s1|	A|	Estradiol treatment|	Estradiol vs DMSO treatment|
| 	s2|	A|	Estradiol treatment|	Estradiol vs DMSO treatment|
| 	s3|	A|	Estradiol treatment|	Estradiol vs DMSO treatment|
| 	s4|	B|	DMSO treatment|	Estradiol vs DMSO treatment|
| 	s4|	B|	DMSO treatment|	Estradiol vs DMSO treatment|
| 	s6|	B|	DMSO treatment|	Estradiol vs DMSO treatment|


`--metric` *METRIC*
: Test statistic to use when computing differential expression. The signal to 
noise metric (S2N) is used by default and is defined as:
S2N = (mean of class A - mean of class B) / (stdev of A + stdev of B)
The following adjustments for low variance are applied when computing the class 
standard deviations:
When the number of samples in a class is < 10, then
	min_stdev = Max(0.025*class_mean, 0.025)
	class_stdev = Max(class_stdev, min_stdev)
else
	The class standard deviation should be at least 0.025
If the metric is 's2n_robust' median and MAD are used in place of the class 
mean and stdev in the formula above. If the metric is 'fc' the fold-change is 
computed as follows:
FC = mean of class A - mean of class B . Default is s2n. Options are 
{s2n|s2n_robust|fc}

`--feature_space` *FEATURE_SPACE*
: Subset to a predefined feature space before performing marker selection. If 
'all' is specified, all features are used without subsetting. Default is all. 
Options are {all|lm|bing|aig|lm_probeset|bing_probeset|full_probeset}

`--feature_id` *FEATURE_ID*
: Custom feature space to use for marker selection, specified as a GRP file of 
feature ids. This flag overrides feature_space.

`--ignore_missing_features` *IGNORE_MISSING_FEATURES*
: Will ignore missing features if true. Default is 0

`--islog2` *ISLOG2*
: Specify if the data is log2 transformed. Default is 1

`--nmarker` *NMARKER*
: Number of markers to select. Default is 100

`--fix_low_var` *FIX_LOW_VAR*
: Adjust for low variance genes. Default is 1

`--min_sample_size` *MIN_SAMPLE_SIZE*
: Minimum number of samples per class. Default is 3

`--use_gctx` *USE_GCTX*
: Output results as binary GCTX files. If false output as text GCT files instead. 
Default is 1

`--skip_rpt` *SKIP_RPT*
: Skip creation of individual folders for each sig_id. Saves time. Default is 0

`--add_heatmap_fields` *ADD_HEATMAP_FIELDS*
: Additional row metadata fields to include in the generated heatmap

## Description
sig_marker_tool compares the expression profiles of two predetermined classes, 
computes differential expression scores and selects the most differentially 
expressed features (markers).
 
Note data is assumed to be in log2 scale. For data in natural scale set the 
--islog2 flag to transform the data before computing the scores.
 
## Examples
 
- Run marker selection on the expression datatset EXP_FILE for the classes 
specified in CLASS_FILE
 
sig_marker_tool('--ds', EXP_FILE, '--phenotype', CLASS_FILE)
 
 

