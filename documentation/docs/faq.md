# Frequently Asked Questions

## What is a SigTool?
A SigTool is a command-line program that allows easy execution of core CMap algorithms and methods via a standardized user interface.

## What are the features / benefits of a SigTool?
Sig Tools provide a reference implementation of core CMap algorithms and facilitate reproducibile analytic workflows and sharing of methods. They also enable easy deployment of tools to production and cloud compute environments. The tools are designed to share common conventions and offer a unified interface such as argument parsing, input validation and unit-tests.

## What SigTools are available?
The Command Reference enumerates the currently available tools organized by function.

## Can I run SigTools w/o configuring a custom analysis environment or access to to Matlab?

We provide Docker [images](https://hub.docker.com/search?q=sig_&type=image) of the sig-tools at DockerHub. These images come pre-configured with all the software dependencies of the tool execute without requiring commercial licences. See [here](docker_demo.md) for a tutorial on how to run custom analyses using Dockerized SigTools.

## Where can I find the source code for sig-tools?

The source code for all Matlab based SigTools is available in the [cmapM](https://github.com/cmap/cmapM) Github repository. 

## How should should I cite these tools in my research?

You can use the following citation:
`
Subramanian, A. et al. A Next Generation Connectivity Map: L1000 Platform and the First 1,000,000 Profiles. Cell 171, 1437–1452.e17 (2017)
`