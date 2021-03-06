# 
# The Computer-Assisted-Structure-Elucidation Kit (CASEkit)
 
Copyright 2017 Christoph Steinbeck

License: MIT, see doc/mit.license

## Introduction

This project hosts various Java classes for teaching and research dealing with spectral data in chemistry and metabolomics.
This project depends on the Chemistry Development Project (CDK), hosted under http://cdk.github.io/
Please refer to these pages for updated information and the latest version of the CDK. CDK's API documentation is available though our [Github site](http://cdk.github.io/cdk/).

## Releases

Latest release of casekit is at https://github.com/steinbeck/casekit/releases/latest

## Download Spectra Source code

This assumes that you have git working on your system and you have initialised your local repository. Refer to https://help.github.com/articles/set-up-git/ for more

Then, downloading spectra is just a matter of

```bash
$ git clone https://github.com/steinbeck/casedk.git
```

## Compiling

Compiling the library is performed with Apache Maven and requires Java 1.7 or later:

```bash
spectra/$ mvn package
```
will create an all-in-one-jar under ./target

## Usage

### Shift Prediction with HOSE codes

The following classes are to demonstrate the prediction of Carbon-13 NMR spectra with HOSE codes. They only demonstrate the basic working principle and ignore, for example, stereochemistry, which can lead to large errors in, for example, the prediction of shifts for E/Z configurations of double bonds. If you want to know more about the details and a sophisticated system implementing them, please refer to Schutz V, Purtuc V, Felsinger S, Robien W (1997) CSEARCH-STEREO: A new generation of NMR database systems allowing three-dimensional spectrum prediction. Fresenius Journal of Analytical Chemistry 359:33–41. doi: 10.1007/s002160050531.

#### NMRShiftDBSDFParser

Takes the NMRShiftDB SDF with assigned spectra (download from help section of NMRShiftDB.org) and produces a Tab-separated file with HOSE codes and assigned shift values. This file can then be read by HOSECodePredictor and SimilarityRanker. 

```bash
usage: java -jar spectra.jar casekit.NMRShiftDBSDFParser -i <arg> -o <arg> [-v] '[-d <arg>]' [-m <arg>]
Generates a table of HOSE codes and assigned shifts from an NMRShiftDB SDF
file from http://nmrshiftdb.nmr.uni-koeln.de/portal/js_pane/P-Help.

 -i,--infile <arg>       filename of NMRShiftDB SDF with spectra
                         (required)
 -o,--outfile <arg>      filename of generated HOSE code table (required)
 -v,--verbose            generate messages about progress of operation
 -d,--picdir <arg>       store pictures in given directory
 -m,--maxspheres <arg>   maximum sphere size up to which to generate HOSE
                         codes
```

#### HOSECodePredictor

Predicts a spectrum (chemical shifts, to be precise) for a given molecule provided as SDF file.  
It needs the TSV file generated by NMRShiftDBSDFParser as input.

```bash
usage: java -jar casekit.jar casekit.HOSECodePredictor -s <arg> -i <arg>
       [-v] [-d <arg>] [-m <arg>]
Predict NMR chemical shifts for a given molecule based on table of HOSE
codes and assigned shifts.

 -s,--hosecodes <arg>    filename of TSV file with HOSE codes (required)
 -i,--infile <arg>       filename of with SDF/MOL file of structures to be
                         predicted (required)
 -v,--verbose            generate messages about progress of operation
 -d,--picdir <arg>       store pictures of structures with assigned shifts
                         in given directory
 -m,--maxspheres <arg>   maximum sphere size up to which to generate HOSE
                         codes. Default is 6 spheres if this option is
                         ommitted.

Please report issues at https://github.com/steinbeck/spectra
```

#### SimilarityRanker

Rank structures based on given experimental spectrum and similarity to
predicted spectrum.

```bash
usage: java -jar casekit.jar casekit.SimilarityRanker -i <arg> -p <arg> -o
       <arg> -s <arg> [-n <arg>] [-v]
Rank structures based on given experimental spectrum and similarity to
predicted spectrum.

 -i,--infile <arg>      filename of with SDF/MOL file of structures to be
                        ranked (required)
 -p,--spectrum <arg>    filename of CSV file with spectrum. Format of each
                        line: <shift>;<number of attached H> (required)
 -o,--outpath <arg>     path to store pictures of ranked output structures
                        (required)
 -s,--hosecodes <arg>   filename of TSV file with HOSE codes (required)
 -n,--number <arg>      number of structures in output file. Default is
                        10, if this option is ommitted
 -v,--verbose           generate messages about progress of operation

Please report issues at https://github.com/steinbeck/spectra
```




