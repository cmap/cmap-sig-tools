---

SigTools are command-line programs that allow easy execution of core CMap algorithms and methods via a standardized user interface. 

This tutorial demonstrates running connectivity analyses on custom datasets in MATLAB using the source code available 
in the [cmapM](https://github.com/cmap/cmapM) Github repository.


We also provide Docker images of the SigTools via DockerHub. 
These images come pre-configured with all the software dependencies for executing the tool on a local environment without 
requiring a commercial MATLAB license. To learn how to use the Docker images see [here](docker_demo.md).

### Pre-requisites for the demo
* Clone the cmapM code repository
```
git clone https://github.com/cmap/cmapM
```
* Configure the MATLAB environment and download test data for the demo
```matlab
% within a MATLAB sesssion type:
cd cmapM
setup
```
* Run all the demos, described in more detail below
```matlab
sig_tool_demos
```

### Connectivity analysis using SigTools

#### 1. Running a Cmap Query against an L1000 dataset using the QueryL1k tool

The QueryL1k tool computes a set-based enrichment similarity between input genesets (aka 
queries) and a small subset of L1000 perturbational gene-expression signatures. 
(Note that while the tool is optimized for datasets generated by the L1000 platform, 
any perturbational dataset can be used).

The algorithm operates as follows. First raw similarity (connectivity) scores 
between a query and CMap signatures are computed. While query methodology is 
agnostic to the specific similarity metric, the default choice is a non-parametric, two-tailed weighted gene-set enrichment score (Subramanian, A. et al. Cell 2017).
 
The raw scores are then scaled (normalized) by the signed-means to allow for 
comparisons across different queries.
 
Finally the statistical significance of the connections adjusted for multiple 
hypotheses is estimated. FDR q-values are estimated by comparing the 
distributions of treatments to null signatures in the dataset.

```matlab  linenums="1"
DATASET_PATH = fullfile(cmapmpath, 'demo-datasets');

% Queries
UP_GENESET = fullfile(DATASET_PATH, 'queries/genesets/dexamethasone_resistance_up.gmt');
DOWN_GENESET = fullfile(DATASET_PATH, '/queries/genesets/dexamethasone_resistance_down.gmt');

% Gene Expression Dataset
% Differential expression score matrix
SCORE_FILE = fullfile(DATASET_PATH, '/l1000/m2.subset.10k/level5_modz.bing_n10000x10174.gctx');
% Corresponding rank matrix
RANK_FILE = fullfile(DATASET_PATH, 'l1000/m2.subset.10k/rank.bing_n10000x10174.gctx');
% Signature annotations
SIG_META_FILE = fullfile(DATASET_PATH, 'l1000/m2.subset.10k/siginfo.txt');
% results folder
OUT_PATH = 'results/queryl1k';

% Run the queryl1k tool
sig_queryl1k_tool('up', UP_GENESET,...
    'down', DOWN_GENESET,...
    'score', SCORE_FILE,...
    'rank', RANK_FILE,...
    'sig_meta', SIG_META_FILE,...
    'max_col', 50000,...
    'create_subdir', 0,...
    'out', OUT_PATH)
``` 
 
**Outputs:** the tool produces the following output (in the `results` folder)
 
`arfs/`: Per-query analysis report files (ARFs)
 
`<QUERY_NAME>/query_result.gct` : a GCT format text file listing the annotations, 
connectivity scores and q-values for each signature in the dataset. The 
following fields are computed by the query tool:
 
- `raw_cs` : Raw connectivity scores
- `norm_cs` : Normalized connectivity score computed by dividing the raw 
connectivity scores by the signed-mean scores of signatures (specified by the 
is_ncs_sig field in the signature metadata file) If the ncs_group field is not 
empty the scores are normalized within each group, otherwise the scores are 
normalized using the global means across all signatures.
- `fdr_q_nlog10` : Negative log10 transformed FDR q-values estimated relative to 
the null signatures (specified by the `is_null_sig` field in the signature 
annotation file).
 
`matrices/query` : Query parameters and result matrices in GCTx format for all 
queries:
 
- `up.gmt`, `dn.gmt`: query genesets in GMT format
- `cs.gctx` : Raw connectivity scores matrix [signatures x queries] 
- `ncs.gctx` : Normalized connectivity score matrix [signatures x queries]
- `fdr_qvalue.gctx` : Estimated false discovery rate q-values [signatures x 
queries]

#### 2. Testing enrichment of user-defined sets using the GSEA Preranked tool

The GSEA Preranked tool computes set-based enrichment analysis against a user-defined 
rank-ordered dataset.  It determines whether a priori defined sets show 
statistically significant enrichment at either end of the ranking.

``` matlab linenums="1"

% Signature sets
SIG_SET = fullfile(DATASET_PATH, 'queries/sigsets/cmap_sig_sets_n9380.gmt');
% Set annotations
QUERY_META_FILE = fullfile(DATASET_PATH, 'queries/sigsets/cmap_sig_sets_info.txt');
% Query result scores (Normalized connectivities)
SCORE_FILE = fullfile(DATASET_PATH, 'pre_computed_results/queryl1k/science_queries/arfs/DEX/query_result.gct');
% results folder
OUT_PATH = 'results/gseapreranked';

% Run the gseapreranked tool
sig_gseapreranked_tool('up', SIG_SET,...
    'score', SCORE_FILE,...
    'query_meta', QUERY_META_FILE,...
    'create_subdir', 0,...
    'out', OUT_PATH);

``` 

#### 3. Querying cell viability data withe the Curie tool using cell-sets
The Curie tool computes a set-based enrichment similarity between input cell-line 
sets (aka queries) and a perturbational cell-fitness signature dataset. Note that while 
the tool is optimized for datasets generated by the PRISM platform, any high-dimensional
cell-fitness dataset can be used.

``` matlab linenums="1"
% Queries (Cell line sets)
CELL_SET = fullfile(DATASET_PATH, 'queries/cellsets/curated_cellsets_n5.gmt');

% Cell viability data
% Cell fitness scores
SCORE_FILE = fullfile(DATASET_PATH, 'prism/repurposing-secondary/level5_modz.zspc.sqrtaud_n1258x489.gctx');
% Corresponding rank matrix
RANK_FILE = fullfile(DATASET_PATH, 'prism/repurposing-secondary/rank_n1258x489.gctx');
% Signature annotations
SIG_META_FILE = fullfile(DATASET_PATH, 'prism/repurposing-secondary/siginfo.txt');

% Run the curie tool
sig_curie_tool('es_tail', 'up',...
    'up', CELL_SET,...
    'score', SCORE_FILE,...
    'rank', RANK_FILE,...
    'sig_meta', SIG_META_FILE,...
    'create_subdir', 0,...
    'out', OUT_PATH);
``` 



