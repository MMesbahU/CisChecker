# CisChecker

## Introduction
CisChecker is a software to check if two mutations in a same gene are located in the same allele (in cis) or not (in trans) using next-generation sequencing data.

## Dependency
### Python
Python (2.7), pysam (0.14.1), pandas(0.16.2) packages
(Other versions are not checked)

## Input files

#### Mutation files

Mutations are provided in a Mutation Annotation Format (MAF) file.

#### Sample sheet of BAM files

Sample list needs be tab-delimited.

Each line contains a sample name, a tab character, and a path to the BAM file of the sample.

```
sample1	/home/usr/bam/sample1.bam
sample2	/home/usr/bam/sample2.bam
```

## How to use
Before usage, type the sentence below.
```
python setup.py install --user

```

```
$ cischecker -h
usage: cischecker [-h] [--version] {listing,cis_check} ...

positional arguments:
  {listing,cis_check}
    listing            Make multiple mutation list from A Mutation Annotation
                       Format (MAF) file (.maf)
    cis_check          merge, compress and index break point file generated by
                       parse function

optional arguments:
  -h, --help           show this help message and exit
  --version            show program's version number and exit

```

CisChecker consists of two steps.

### 1, make multiple mutation lists

First, make multiple mutation file from MAF file.

Multiple mutation file returns lists with multiple mutations in a same gene in a same sample.

If  there are more than three mutations in a same gene in a same sample, list of all two combinations will be returned.

```
$ cischecker listing -h
usage: cischecker listing [-h] [-g GENE] [-s SAMPLE] [-v] input.maf output_file

positional arguments:
  input.maf             path to input MAF file
  output_file           path to output file

optional arguments:
  -h, --help            show this help message and exit
  -g GENE, --gene GENE  specify the gene you want to check (if you want to choose multiple genes, separate them with colon) (default: all genes)
  -s SAMPLE, --sample SAMPLE
                        specify the sample you want to check (if you want to choose multiple samples, separate them with colon) (default: all samples)
  -v, --vaf             use VAF (variant allele fraction) information
```

Example
```
$ cischecker listing example/test_maf.txt example/test_list.txt
```


### 2, check if multiple mutations are in cis or not
Second, check if multiple mutations are located in cis.

```
$ cischecker cis_check -h
usage: cischecker cis_check [-h] [--baseq BASEQ] [--mapq MAPQ] [--overlaps] [--stepper {all,nofilter,samtools}] [--orphans] [-v] [-m] [-d] multiple_mutation_list sample_list.txt output_file

positional arguments:
  multiple_mutation_list
                        path to multiple mutation file (generated by listing function)
  sample_list.txt       path to sample list
  output_file           path to output file

optional arguments:
  -h, --help            show this help message and exit
  --baseq BASEQ         Minimum base quality (samtools option; default = 0) (default: 0)
  --mapq MAPQ           Minimum mapping quality (samtools option; default = 0) (default: 0)
  --overlaps            If set to True, detect if read pairs overlap and only take the higher quality base (samtools option; default True) (default: True)
  --stepper {all,nofilter,samtools}
                        The stepper controls how the iterator advances. (samtools option; default all) (default: all)
  --orphans             Ignore orphans (paired reads that are not in a proper pair) (samtools option; default True) (default: True)
  -v, --vaf             use VAF (variant allele fraction) information (default: False)
  -m, --mapping_difference
                        Indel mapping differs between mapping tools. If MAF file and BAM file were mapped with a different mapping tool (or same tool with different parameters), use this argument. (default: False)
  -d, --debug           debug function (default: False)
```

Example
```
$ cischecker cis_check example/test_list.txt example/sample_list.txt example/test_output.txt
```
