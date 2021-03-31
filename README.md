# psytrans.py - Parasite & Symbiont Transcriptome Separation

## Description

psytrans.py separates the sequences of a host species from those of its main symbiont(s) or parasite(s) based on Support Vector Machine classification.
The program takes as input a file in fasta format with the sequences to be classified.
The program also requires a file with sequences of a species related to the host, and a file with sequences related to the symbiont (or parasite).
The queries will be compared to these two files using BLASTX.
Alternatively, the user can provide the output of pre-computed BLASTX searches (in tabular format: -outfmt 6 or 7).
The classification is then carried out using the command line tools from libsvm.

## Dependencies

psytrans requires makeblastdb and blastx from the NCBI blast+ distribution, unless the user provides pre-computed blast results.
psytrans also requires a few command line utilities from libsvm: svm-scale, svm-train and svm-predict

## Installation

### Conda

A bioconda package is available for intalling psytrans and all its dependencies. 
Steps:
1- Create an environemnt for psytrans
```console
conda create -n psytrans -y
```
2- Activate envrironment
```console
conda activate psytrans
```
3- Install
```console
conda install -c bioconda psytrans
```
### Docker

Using docker
```console
docker pull quay.io/biocontainers/psytrans:2.0.0--0
```

### Singularity

Using singularity image
```console
singularity run https://depot.galaxyproject.org/singularity/psytrans:2.0.0--0
```

## Options

**Generic Program Information**
```
       -h, --help
              Print a usage message briefly summarizing the command-line options.

   **Global options**

       -R, --restart
              Restart the script from the last checkpoint.

       -p, --nbThreads
              Number of threads to use for the blast searches and for the SVM training.

       -V, --verbosemode
              Runs the script in verbose mode.

       -t, --tempDir
              Specify the name of the temporary directory.

       -X, --clearTemp
              Clears all temporary data in the temporary directory upon completion.

       -z, --stopAfter
              Choices:['db','runBlast','parseBlast','kmers','SVM']
              This option allows the user to choose whether the process should stop, once the process has completed a specific stage.
              db refers to the database creation stage;
              runBlast refers to the BLAST search stage;
              parseBlast refers to the separation of unambiguous and ambiguous sequence stage;
              kmers refers to the preparation of SVM input stage;
              SVM refers to the SVM training and testing stage.
```
**Preparation of training set options**
```
       -e, --maxBestEvalue
              Set the maximum value for the best e-value to be used to classify unambiguous sequences.

       -n, --numberOfSeq
              Set the maximum number of training & testing sequences.
```
**Kmer parameters**
```
       -c, --minWordSize
              Set the minimum value of DNA word length.

       -k, --maxWordSize
              Set the maximum value of DNA word length.
```

## Examples

You start with an assembly containing a mixture of sequences from a host A and a symbiont (or parasite) B: host_and_symb.fasta
You also provide a file with proteins from a species related to the host: related_host_proteins.fasta
and a file with proteins from a species related to the symbiont: related_symb_proteins.fasta
You can then start the Psytrans process as follows:
```
python psytrans.py host_and_symb.fasta -H related_host_proteins.fasta -S related_symb_proteins.fasta
```
Wait a few hours (depending on the number of sequences), and in the current directory you will find a new file starting with the prefix `host_` and another file starting with the prefix `symb_`, corresponding to the sequences classified as host or symbiont (or parasite) respectively.

To run the program using 8 threads and using /home/user/tmp as a temporary directory, use the following command:
```
python psytrans.py host_and_symb.fasta -H related_host_proteins.fasta -S related_symb_proteins.fasta -p 8 -t /home/user/tmp
```

## Authors

Psytrans was developed by Sylvain Forêt and Jue-Sheng Ong. This repository modified the script to use python3. Original work can be found at <https://github.com/sylvainforet/psytrans>  
Modifications by Nicola Conci and Ramon Rivera

## Reporting bugs

We could help in some issues since we used the tool frequently in the lab.   
However, it should be noted that we are not going to be actively developing psytrans.  
Since the original repository is not being used anymore you could open issues here if needed. 

Original issues at Psytrans repository: https://github.com/sylvainforet/psytrans

## COPYRIGHT

Copyright © 2021 Nicola Conci & Ramón Rivera Vicéns.  
Copyright © 2014 Sylvain Forêt & Jue-Sheng Ong.

psytrans  is a free  software and comes with ABSOLUTELY NO WARRANTY.  You are welcome to redistribute it under the terms of the GNU General Public License
versions 3 or later.  For more information about these matters see http://www.gnu.org/licenses/.
