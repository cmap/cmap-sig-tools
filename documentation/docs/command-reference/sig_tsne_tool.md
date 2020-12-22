# sig_tsne_tool
Run T-SNE on a dataset

## Synopsis
`sig_tsne_tool` [--ds DS] [--ds_meta DS_META] 
[--is_pairwise IS_PAIRWISE] [--cid CID] [--rid RID] [--row_space ROW_SPACE] [--sample_dim 
SAMPLE_DIM] [--out_dim OUT_DIM] [--algorithm ALGORITHM] [--initial_dim INITIAL_DIM] 
[--perplexity PERPLEXITY] [--theta THETA] [--missing_action MISSING_ACTION] 
[--missing_fill_value MISSING_FILL_VALUE] [--disable_table DISABLE_TABLE]

## Arguments

`--ds` *DS*
: Input dataset

`--ds_meta` *DS_META*
: Optional annotations as a TSV table for the input dataset for the dimension 
being operated on. The first column must match the corresponding id field in ds

`--is_pairwise` *IS_PAIRWISE*
: Handles the input dataset as a distance or similarity matrix if true. Expects 
the the input to be square and symmetric. Assumes the values are similarities 
if the main diagonal is one or distances if the main diagonal is zero. Skips 
the initial dimensionality reduction (via PCA) and pairwise euclidean distance 
computation and uses the tsne_d algorithm to perform the low-dimensional 
embedding. Default is 0

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

`--sample_dim` *SAMPLE_DIM*
: Sample dimension of the dataset. Default is column. Options are 
{1|2|column|row}

`--out_dim` *OUT_DIM*
: Output dimensionality. Default is 2

`--algorithm` *ALGORITHM*
: The t-SNE implemention to use. The standard algorithm is a native matlab 
implementation that is appropriate for small to moderate sized datasets and if 
more than 2 output dimensions are required. The Barnes Hut algorithm is a fast 
C++ implementation suitable for 2D tSNE representation of large datasets (>5000 
samples).. Default is auto. Options are {auto|standard|barnes-hut}

`--initial_dim` *INITIAL_DIM*
: Initial number of PCA dimensions to use. Default is 50

`--perplexity` *PERPLEXITY*
: Perplexity is a measure for information that is defined as 2 to the power of 
the Shannon entropy. It may be viewed as a tuning parameter that sets the 
number of effective nearest neighbors. It is comparable to the number of 
nearest neighbors k that is employed in many manifold learners.
The performance of t-SNE is fairly robust under different settings of the 
perplexity. The most appropriate value depends on the density of the data. In 
general a denser dataset requires a larger perplexity. Typical values for the 
perplexity range between 5 and 50. Default is 30

`--theta` *THETA*
: Used only in the Barnes-Hut implementation. It's a trade-off parameter to 
choose between speed and accuracy: theta = 0 corresponds to standard, slow 
t-SNE, while theta = 1 makes very crude approximations. Appropriate values for 
theta are between 0.1 and 0.7. Default is 0.5

`--missing_action` *MISSING_ACTION*
: Action to take if data contains missing values. If 'drop' is specified the 
entire column (or row if sample_dim='row') is excluded prior to analysis. If 
'impute' is specified the missing values are replaced by row means (or column 
means if sample_dim='row'). If 'fill' is specified, missing values are replaced 
with 'missing_fill_value'. Default is none. Options are {none|drop|impute|fill}

`--missing_fill_value` *MISSING_FILL_VALUE*
: Replace missing data with specified value if the 'fill' option is specified for 
missing_action. Default is 0

`--disable_table` *DISABLE_TABLE*
: Disable generating annotated text table for first two TSNE components. The 
table can be generated post-hoc from the saved tsne.gctx matrix if needed.. 
Default is 0

## Description
Applies t-distributed stochastic neighbor embedding (t-SNE) to high dimensional 
datasets and returns a 2-d mapping of datapoints. t-SNE is a dimensionality 
reduction technique that is particularly well suited for visualization of high 
dimensional data in 2 or 3 dimensions.
 
For datasets with <= 5000 samples, the standard t-SNE algorithm is used. For 
larger datasets the Barnes-HUT algorithm is employed.
 
For details see http://homepage.tudelft.nl/19j49/t-SNE.html
 
## Examples
 
- tSNE with default parameters
 
`sig_tsne_tool --ds 'x.gctx`
 
- tSNE along rows of a large dataset with >5000 rows
 
`sig_tsne_tool --ds 'large.gctx' --dim row --algorithm barnes-hut`
 
 

