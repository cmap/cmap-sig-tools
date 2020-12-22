# sig_testmlr_tool
Apply given model to predict genes using multiple linear regression.

## Synopsis
`sig_testmlr_tool` [--ds DS] [--model MODEL] 
[--minval MINVAL] [--maxval MAXVAL] [--use_gctx USE_GCTX] [--xform XFORM]

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

`--ds` *DS*
: GCT or GCTX, Input dataset (genes x samples), minimally containing all 
landmarks genes specified in the model. Non-landmark genes will be ignored

`--model` *MODEL*
: MLR model output from SIG_TRAINMLR_TOOL to utilize to infer dependent genes

`--minval` *MINVAL*
: Minimum value for output dataset. Lower values will be replaced with this 
value. Default is 0

`--maxval` *MAXVAL*
: Maximum value for output dataset. Greater values will be replaced with this 
value. Default is 15

`--use_gctx` *USE_GCTX*
: Outputs text GCT files if false. Default is 1

`--xform` *XFORM*
: String. Specify any transformations to apply to data prior to training. Default 
is none. Options are {none|log2|abs|pow2|zscore}

## Description
 The sig_testmlr_tool takes as input a test dataset (genes x samples) minimally 
containing all landmark genes and an MLR model (output by SIG_TRAINMLR_TOOL) 
and output inferred values for dependent genes by applying the model. Note that 
the values in the input dataset must match the data used for training. The 
standard L1000 analytical workflow employs normalized log2 transformed 
expression as input to the model. The --xform argument can be optionally 
specified to transform the input data. The output is saved as a GCTX file with 
the inferred dependent genes appended to the original landmark data.
 
## Examples
 
- Apply model to landmark data.
 
`sig_testmlr_tool --ds 'test_data.gctx' --model '/filepath/model.gctx'`
 
- Apply model to landmark data retricting the minimum value in the output to 2.
 
`sig_testmlr_tool --ds 'test_data.gctx' --model '/filepath/model.gctx' --minval 2`
 
 

