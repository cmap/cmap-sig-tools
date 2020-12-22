# sig_trainmlr_tool
Create a model given a training dataset using multilinear regression.

## Synopsis:
`sig_trainmlr_tool` [--ds DS] [--modeltype 
MODELTYPE] [--grp_landmark GRP_LANDMARK] [--dependents DEPENDENTS] [--cid CID] [--xform 
XFORM] [--precision PRECISION] [--outfmt OUTFMT]

## Arguments

`--ds` *DS*
: Input dataset that includes both landmarks and resulting outputs.

`--modeltype` *MODELTYPE*
: String. Regression model to be used:
pinv_int - Linear Regression with an intercept.
pinv - Linear Regression. All best fits lines pass through origin.
. Default is pinv_int. Options are {pinv_int|pinv}

`--grp_landmark` *GRP_LANDMARK*
: GRP file containing identifiers for landmark genes. Must correspond to the 
row-ids in DS

`--dependents` *DEPENDENTS*
: GRP file containing identifiers for dependent genes to infer. Must correspond 
to row-ids in DS and be mutually exclusive from the landmarks.

`--cid` *CID*
: GRP file of column ids (samples) in DS to use for training.

`--xform` *XFORM*
: String. Specify any transformations to apply to data prior to training. Default 
is none. Options are {none|log2|abs|pow2|zscore}

`--precision` *PRECISION*
: Integer. Level of precision (decimal places) for output model weights.. Default 
is 6

`--outfmt` *OUTFMT*
: Model Output format. Default is gctx. Options are {mat|gct|gctx}

## Description
This tool takes as input a training dataset (genes x samples) 
containing expression values for a specified set of predictor (landmark) genes 
and some number of dependent genes and builds a multiple linear regression 
(MLR) model. The model can be applied to a new dataset using the 
SIG_TESTMLR_TOOL to infer the expression of the dependent genes based on 
landmark expression.
 
## Examples
 
- Create a model from training data and landmark GRP file.
 
`sig_trainmlr_tool --ds 'training_data.gctx' --grp_landmark 'landmark.grp'`
 
- Create a model for a subset of dependent genes.
 
`sig_trainmlr_tool --ds 'training_data.gctx' --grp_landmark 'landmark.grp' --dependents 'subset.grp'`
 
- Perform z-scoring prior to creating model.
 
`sig_trainmlr_tool --ds 'training_data.gctx' --grp_landmark 'landmark.grp' --xform 'zscore'`
 
 

