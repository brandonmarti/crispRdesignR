# crispRdesignR
Software used to design guide RNA sequences for CRISPR/Cas9 genome editing

This software aims to provide all scientifically pertinent information when designing guide RNA sequences for Cas9 genome editing. When provided a target DNA sequence for editing, a genome to check for off-targets in, and a genome annotation file (.gtf) to provide addition information about off-target matches it will out put information for two separate data tables. The first table contains all information on the generated sgRNA themselves (sgRNA sequence, PAM, Direction, Start, End, GC content, Presence of Homopolymers, Self Complementarity, Effciency Score (Doench 2016), and Genomic Matches). The second table contains all information on the found off-target sequences (Original sgRNA Sequence, Chromosome, Start, End, Number of Mismatches, Direction, CFD Scores, Matched Sequence, Gene ID, Gene Name, Sequence Type, and Exon Number). Additionally, a user may provide their own DNA libraries to search for off targets in and use a genome annotation file of their preference.

![crisprdesignrtables](https://user-images.githubusercontent.com/38253997/47939240-f591ef80-debc-11e8-8d33-75d2eb010859.png)

## Installation and dependencies

Steps to install crispRdesignR (tested in R version 3.4.4):

##### dependencies gbm, vtreat and stringr:

`install.packages("gbm")`

`install.packages("vtreat")`

`install.packages("stringr", repos='http://cran.us.r-project.org')`

##### dependencies shiny, for the user interface

`install.packages("shiny")`

##### dependencies BioStrings and BSgenome packages through Bioconductor:

`source("https://bioconductor.org/biocLite.R")`

`biocLite("Biostrings")`

`biocLite("BSgenome")`

`biocLite("AnnotationHub")`

##### Your genome(s) of interest from BSgenome. The example dataset uses the yeast genome

`biocLite("BSgenome.Scerevisiae.UCSC.sacCer2")`

`library("BSgenome.Scerevisiae.UCSC.sacCer2")`

##### Install crispRdesignR, where "path_to_directory" is the path of the decompressed crispRdesignR-master folder

`install.packages(path_to_directory, repos = NULL, type="source")`

## Quick start with the GUI

Example Data is located in /inst/ folder.

The DAK1.fasta and DAK1_short.txt file contains a DNA sequence native to the DAK1 gene that can be copied and pasted into crispRdesignR or uploaded as a file (in the GUI version).

The "Saccharomyces_cerevisiae.R64-1-1.92.gtf.gz" file is the compressed genome annotation file for Saccharomyces cerevisiae. Both compressed and uncompressed .gtf files can be used.

Using the GUI version:

- `library(shiny)`

- `library(crispRdesignR)`

- `crispRdesignRUI()`

- Click on the “UseFASTA or txt file as target sequence” button and choose the DAK1.fasta or DAK1_short.txt file, or copy and paste the sequence in the box.

- select the Saccharomyces cerevisiae genome

- browse to choose the .gtf file Saccharomyces_cerevisiae.R64-1-1.92.gtf.gz

- click on the Find sgRNA button

![crisprdesignrss](https://user-images.githubusercontent.com/38253997/47938309-10169980-deba-11e8-83de-c00e4a6b72cd.PNG)

Additional Genome annotation files can be found here: https://useast.ensembl.org/info/data/ftp/index.html

Note: Even though it might be possible to select them in the GUI, Genomes must be installed (with `install.packages(BSgenome.yourgenome)`) and activated (with `library(yourgenome)` before they can be used in the shiny app.

## Command-line version

- Load crispRdesignR with `library(crispRdesignR)` and ensure that your genome of interest is installed and loaded. The following commands can be used to output the same data tables as in the GUI version.

##### sgRNA Design

All data can be generated without the graphic interface by using a single function: `sgRNA_design(userseq, genomename, gtfname, userPAM, calloffs = TRUE, annotateoffs = TRUE)`

`userseq`: The target sequence to generate sgRNA guides for. Can either be a character sequence containing DNA bases or the name of a fasta/text file in the working directory.

`genomename`: The name of a genome (in BSgenome format) to check for off-targets in. These genomes can be downloaded through BSgenome or compiled by the user.

`gtfname`: The name of a genome annotation file (.gtf) in the working directory to check off-target sequences against.

`userPAM`: An optional argument used to set a custom PAM for the sgRNA. If not set, the function will default to the "NGG" PAM. Warning: Doench efficieny scores are only accurate for the "NGG" PAM.

`calloffs`: If TRUE, the function will search for off-targets in the genome chosen specified by the genomename argument. If FALSE, off-target calling will be skipped.

`annotateoffs`: If TRUE, the function will provide annotations for the off-targets called using the genome annotation file specified by the gtfname argument. If FALSE, off-target annotation will be skipped.

##### sgRNA Data Retrieval

The data on the generated sgRNA sequences can be retrieved with: `getsgRNAdata(x)`

`x`: The raw data generated by `sgRNA_design()`

##### Off-Target Data Retrieval

The additional off-target data can be retrieved with `getofftargetdata(x)`

`x`: The raw data generated by `sgRNA_design()`

##### sgRNA Design Doench 2014 (outdated)

An older version of the sgRNA_design function: `sgRNA_design_Doench2014(userseq, genomename, gtfname, userPAM, calloffs = TRUE, annotateoffs = TRUE)`

Uses same arguments as `sgRNA_design`

###### Example:
`alldata <- sgRNA_design("GGCAGAGCTTCGTATGTCGGCGATTCATCTCAAGTAGAAGATCCTGGTGCAGTAGG", BSgenome.Scerevisiae.UCSC.sacCer2, "Saccharomyces_cerevisiae.R64-1-1.92.gtf.gz")`

`sgRNAdata <- getsgRNAdata(alldata)`

`offtargetdata <- getofftargetdata(alldata)`

###### Example:
`exampledata <- sgRNA_design("DAK1.fasta", BSgenome.Scerevisiae.UCSC.sacCer2, "Saccharomyces_cerevisiae.R64-1-1.92.gtf.gz", "NAG", calloffs = TRUE, annotateoffs = FALSE)`

## Example Data

Example Data is located in /inst/ folder. To use the DAK1.fasta file, place it in the working directory and refer to it in crispRdesignR. The DAK1_short.txt file contains a short DNA sequence that can be copied and pasted into crispRdesignR. Both sequences are native to the DAK1 gene in Saccharomyces cerevisiae. The "Saccharomyces_cerevisiae.R64-1-1.92.gtf.gz" file is a genome annotation file for Saccharomyces cerevisiae and must also be placed in the working directory.
