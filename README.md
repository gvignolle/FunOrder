FunOrder
=========

The Functional Order (FunOrder) tool - Identification of essential biosynthetic genes through computational molecular co-evolution – searches for co-evolutionary linked genes in the biosynthetic gene clusters (BGCs). As co-localization and co-expression have been used for the detection of BGCs, we demonstrated that co-evolution can be promoted to determine which genes to choose for heterologous expression. Due to the flexibility and adaptability of the core program it is possible to intergate any given protein database. We aim to develop further databases to be integrated into our robust FunOrder tool. As a future perspective, given the increase in computational resources, FunOrder might be used for the analysis of co-evolution on a whole proteome, enabling the genome wide detection of evolutionary linked genes. Making a first step in tackling the issue of fungal genes involved in SM production dispersed over the genome and thereby undetectable by conventional software tools searching for clustering genes.

The Functional Order (FunOrder) tool - Identification of essential biosynthetic genes through computational molecular co-evolution. FunOrder is copyright 2020 Gabriel A. Vignolle, Robert L. Mach, Astrid R. Mach-Aigner and Christian Derntl, and is released under the MIT License. If you find FunOrder useful to your work, please cite:

Vignolle, G. A.; Mach, R. L.; Mach-Aigner, A. R.; Derntl, C. The Functional Order (FunOrder) tool: Identification of essential biosynthetic genes through computational molecular co-evolution. **submitted to** *Nucleic Acids Research*, **2020**.  **unpublished**


The software input files are biosynthetic gene clusters (BGC) with gene translations in genbank file format or fasta format, that contain the amino acid sequences of all the genes found in the BGC of interest. In case a genbank file is provided a python script (Genbank to FASTA by Cedar McKay and Gabrielle Rocap, University of Washington) is called to extract the amino acid sequence of the genes in the BGC and create a fasta file from these. 
The multi-fasta file is then split in individual fasta files containing a single sequence. These are placed in a subfolder created for the analysis of the BGC. Each file is either named after the position of the gene in the BGC or after the respective protein sequence description or gene identification. This varies from the input file and the varying annotations used. Each header of the query sequences is tagged with the identifier "query" at the beginning of the header. After this step the sequences are used to query our manually curated database of fungal proteomes  performing sequence similarity search using blastp 2.8.1+ (Protein-Protein BLAST) to search the database. The output of this search is saved in a file with the ".tab" extension, the output format is the classical tab delimited BLAST output. Furthermore the user has the option to activate a remote search of the non-redundant National Center for Biotechnology Information (NCBI) protein database, for homologous sequences, the output is saved as described above in a file with the "ncbi.tab" extension. This was included as a preliminary analysis of the inputted sequences and to facilitate downstream analysis and annotation of the BGC. 
The top 20 sequences from the database search from the manually curated fungal proteome database are extracted and concatenated with the query sequence in one file. A customized Perl script removes the duplicate entries in the file if any were introduced. Using emma a multiple sequence alignment of the protein sequences based on the ClustalW algorithm is calculated and a dendrogram computed. Using RAxML (Randomized Axelerated Maximum Likelihood) first 100 rapid Bootstraps are performed and then a search for the best-scoring maximum likelihood (ML) tree. These phylogenetic trees are computed for each gene in the BGC using the LG amino acid substitution model. Furthermore a standard ascertainment bias correction by Paul O. Lewis is performed. Then after inferring the ML trees from the multiple sequence alignment the strict distance and speciation distance between all inferred trees is computed using the TreeKO algorithm. The tool compares the topology of different trees; a distance of 0 in both distance measures represents identical trees. In this context, a higher similarity between the different trees of the individual genes points towards a shared evolution. The **strict distance** is a weighted Robinson-Foulds (RF) distance measure that penalizes dissimilarities in evolutionarily important events such as gene losses and gene duplications. Furthermore the strict distance has been suggested to be more significant in the detection of co-evolution. In contrast, the speciation or **evolutionary distance** is computed without taking evolutionary exceptions, such as duplication events or different species content of the two compared trees, into account and infers shared "speciation history" based solely on topology without considering branch lengths and only considering shared species of the compared trees. Therefore, an evolutionary distance of 0 does not necessarily describe identical trees but shared "speciation history" of shared species. The comparison and analysis of the two distance measures is made possible through a custom BASH script and a special identifier introduced in the fungal proteome database. This enables a more comprehensive picture of the potential co-evolution within the analysed BGC. All pairwise strict and evolutionary distances are saved as matrices respectively. The matrices are used as input for an R script for visualization and further analysis of the distances. The strict and evolutionary distances are summed up to a third combined distance measure. 

The three distance matrices are first visualized as heatmaps with a dendrogram computed with the complete linkage method, that finds similar clusters. Then the Euclidean distance within the matrices is computed and clustered using Ward’s minimum variance method aiming at finding compact spherical clusters, with the implemented squaring of the dissimilarities before cluster updating, for each of the three distance matrices separately, with scaled and unscaled input data. Lastly a principle component analysis (PCA) is performed on each distance matrix and the score plot of the first two principle components visualized, respectively. FunOrder includes scripts adapted to the use on servers and for the integration in various pipelines.


Dependencies
------------

Third party programs

* [Python](https://www.anaconda.com)
* [Perl](https://www.perl.org/get.html)
* [R](https://www.r-project.org/)
* [Emboss](http://emboss.sourceforge.net/download/)
* [RAxML](https://github.com/stamatak/standard-RAxML)

Python packages:

* [ete2](https://pypi.org/project/ete2/)

R packages:

* readr
* stats
* gplots
* car
* mdatools

Installation
------------

These instructions should work on [Debian](https://www.debian.org)-based linux distributions such as [Ubuntu](https://www.ubuntu.com).

First we install the EMBOSS package according to the instructions.
Then we install RAxML according to the instructions.
After R, Perl and Python is installed, install the ete2 package.

```
pip install ete2
```

Now install the R packages if not already installed.

```
install.packages(readr) # at the R prompt
install.packages(stats) # at the R prompt
install.packages(gplots) # at the R prompt
install.packages(car) # at the R prompt
install.packages(mdatools) # at the R prompt
```

Now download FunOrder **funorder_v1.tar.xz** and unpack the archive.

```
tar -xf funorder_v1.tar.xz
```

open the scripts funorder.sh ; funorder_fasta_only.sh ; funorder_server.sh ; funorder_server_fasta_only.sh
change in line 38 in funorder.sh ; funorder_fasta_only.sh and line 40 in funorder_server.sh ; funorder_server_fasta_only.sh:

```
SOURCEDIR=~/funorder_proj/funorder_v1/ 
```
to
```
SOURCEDIR=~/path/to/your/directory/funorder_v1/
```

You can now add the FunOrder/pipeline directory to your $PATH environmental variable.
Alternativeley you can call the FunOrder/pipeline directory directly.




### 1) using FunOrder in default mode with Genbank files
------------

```
sh ~/path/to/directory/funorder_v1/funorder.sh [Thread number] [gbk file] [outputdirectory] [database]
```

or if you added the FunOrder/pipeline directory to your $PATH environmental variable.

```
sh funorder.sh [Thread number] [gbk file] [outputdirectory] [database]
```

following folders will be created in the chosen outputdirectory:

1. file.gbk.analysis
2. file.gbk.analysis/alignment
3. file.gbk.analysis/hits

The output of the optional BLAST analysis is saved in the /file.gbk.analysis directory.
The output of FunOrder is saved in /file.gbk.analysis/alignment

#### Output files produced by funorder.sh

File                         | Description
-----------------------------|------------
Rplot.pdf                    | PDF file with the Analyze.R output as described in our publication
strict_distance.matrix       | matrix of the strict distance
evol_distance.matrix         | matrix of the evolutionary [speciation] distance




### 2) using FunOrder in default mode with fasta files
------------

```
sh ~/path/to/directory/funorder_v1/funorder_fasta_only.sh [Thread number] [fasta file] [outputdirectory] [database]
```

or if you added the FunOrder/pipeline directory to your $PATH environmental variable.

```
sh funorder_fasta_only.sh [Thread number] [fasta file] [outputdirectory] [database]
```

following folders will be created in the chosen outputdirectory:

1. file.fasta.analysis
2. file.fasta.analysis/alignment
3. file.fasta.analysis/hits

The output of the optional BLAST analysis is saved in the /file.gbk.analysis directory.
The output of FunOrder is saved in /file.fasta.analysis/alignment

#### Output files produced by funorder_fasta_only.sh

File                         | Description
-----------------------------|------------
Rplot.pdf                    | PDF file with the Analyze.R output as described in our publication
strict_distance.matrix       | matrix of the strict distance
evol_distance.matrix         | matrix of the evolutionary [speciation] distance




### 3) using FunOrder in server mode with gbk files
------------

```
sh ~/path/to/directory/funorder_v1/funorder_server.sh [Thread number] [gbk file] [outputdirectory] [database]
```

or if you added the FunOrder/pipeline directory to your $PATH environmental variable.

```
sh funorder_server.sh [Thread number] [gbk file] [outputdirectory] [database]
```

following folders will be created in the chosen outputdirectory:

1. file.gbk.analysis
2. file.gbk.analysis/alignment
3. file.gbk.analysis/hits

The output of FunOrder is saved in /file.gbk.analysis/alignment

#### Output files produced by funorder.sh

File                         | Description
-----------------------------|------------
Rplot.pdf                    | PDF file with the Analyze.R output as described in our publication
strict_distance.matrix       | matrix of the strict distance
evol_distance.matrix         | matrix of the evolutionary [speciation] distance




### 4) using FunOrder in server mode with fasta files
------------

```
sh ~/path/to/directory/funorder_v1/funorder_server_fasta_only.sh [Thread number] [fasta file] [outputdirectory] [database]
```

or if you added the FunOrder/pipeline directory to your $PATH environmental variable.

```
sh funorder_server_fasta_only.sh [Thread number] [fasta file] [outputdirectory] [database]
```

following folders will be created in the chosen outputdirectory:

1. file.fasta.analysis
2. file.fasta.analysis/alignment
3. file.fasta.analysis/hits

The output of FunOrder is saved in /file.fasta.analysis/alignment

#### Output files produced by funorder_fasta_only.sh

File                         | Description
-----------------------------|------------
Rplot.pdf                    | PDF file with the Analyze.R output as described in our publication
strict_distance.matrix       | matrix of the strict distance
evol_distance.matrix         | matrix of the evolutionary [speciation] distance


