# Shared Arguments
These arguments are common to all tools

## Synopsis:

`sig_*_tool` [--help, -h] [--undef_action UNDEF_ACTION] [-o, --out OUT] [--runtests]
[--rundemo] [--rpt RPT] [--create_subdir CREATE_SUBDIR] [--verbose VERBOSE] [--encode_url
ENCODE_URL] [--config CONFIG] 

## Arguments:

`--help, -h`
: Show this help message and exit

`--undef_action` *UNDEF_ACTION*
: Action to take if an undefined argument is encountered. Default is error.
Options are {error|warn|ignore}

`-o`, `--out` *OUT*
: Output path

`--runtests`
:	Execute functional and unit tests. Default is 0

`--rundemo`
:	Run the tool with sample inputs. Default is 0

`--rpt` *RPT*
:	Report folder prefix

`--create_subdir` *CREATE_SUBDIR*
:	Create subfolder within out for saving results. Default is 1

`--verbose` *VERBOSE*
:	Print debugging messages. Default is 1

`--encode_url` *ENCODE_URL*
:	Encode all input URLs. Default is 0

`--config` *CONFIG*
:	Argument configuration file

