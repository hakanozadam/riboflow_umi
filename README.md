# RIBOFLOW with UMIs

This is a preliminary version of RiboFlow that can process libraries prepared with Unique Molecular Identifiers (UMIs).

RiboFlow extracts UMIs and stores them in the Fastq headers and uses the UMIs in deduplication ( instead of position based read collapsing ). For this purpose RiboFlow uses umi_tools.

## Existing Users of RiboFlow:
Install [umi_tools](https://umi-tools.readthedocs.io/en/latest/). 

```
conda install -c bioconda -c conda-forge umi_tools
```

Note that umi_tools is not in the Docker image of RiboFlow yet (coming soon though). So DO NOT RUN Riboflow using the Docker Image for UMI runs.

## Project File:
See also `project.yaml` in this repo.

The following parameter must be set:
```
dedup_method: "umi_tools"
```

Also, depending on the organization of the UMI sequences, users must set the following two parameters:

```
umi_tools_extract_arguments: "-p \"^(?P<umi_1>.{12})(?P<discard_1>.{4}).+$\" --extract-method=regex"
umi_tools_dedup_arguments:   "--read-length"
```

The above example takes the first 12 nucleotides from the 5' end, discards the 4 nucleotides downstream and writes the 12 nt UMI sequence to the header.
The second parameter tells umi_tools to use read lengths IN ADDITION to UMI sequencing in collapsing reads. Note that these two arguments are directly provided to umi_tools. So users are encouraged to familirize themselves with [umi_tools](https://umi-tools.readthedocs.io/en/latest/).

## RiboFlow.groovy file

RiboFlow.groovy file has been updated to incorporate umi_tools. So existing users need to replace their RiboFlow.groovy
 with the new one in this repository.
 
 ## RNA-Seq support
 
 In the current version UMIs for only ribosome profiling libraries are supported. So RNA-Seq libraries can either be used without deduplication or the reads can be collapsed based on position.
