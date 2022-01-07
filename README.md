
FunOrder 2
==========

The Functional Order (FunOrder) tool - Identification of essential biosynthetic genes through computational molecular co-evolution – searches for co-evolutionary linked genes in a set of inputted genes. The functionality and applicability was tested with biosynthetic gene clusters (BGCs). The resulting information can be used to choose which genes of a gene cluster are most likely the core genes necessary for the biosynthesis of a secondary metabolite. The flexibility and adaptability of the core program allows the integration of any protein database and can thus be adapted for different phyla and research objectives. FunOrder might be used for the analysis of co-evolution on a whole proteome, enabling the genome wide detection of evolutionary linked genes.

The Functional Order (FunOrder) tool - Identification of essential biosynthetic genes through computational molecular co-evolution. FunOrder is copyright 2020 Gabriel A. Vignolle, Denise Schaffer, Robert L. Mach, Astrid R. Mach-Aigner and Christian Derntl, and is released under the MIT License. If you find FunOrder useful to your work, please cite:

**FunOrder 2.0 – a fully automated method for the identification of co-evolved genes**

https://zenodo.org/record/5118984 and DOI: 10.5281/zenodo.5118984 for the code and

Vignolle GA, Schaffer D, Zehetner L, Mach RL, Mach-Aigner AR, Derntl C (2021) **FunOrder: A robust and semi-automated method for the identification of essential biosynthetic genes through computational molecular co-evolution.** PLoS Comput Biol 17(9): e1009372. doi: https://doi.org/10.1371/journal.pcbi.1009372

**The Functional Order (FunOrder) tool - Identification of essential biosynthetic genes through computational molecular co-evolution** Gabriel A Vignolle, Denise Schaffer, Robert L Mach, Astrid R Mach-Aigner, Christian Derntl. **bioRxiv** 2021.01.29.428829; doi: https://doi.org/10.1101/2021.01.29.428829

The software input files are biosynthetic gene clusters (BGC) with gene translations in genbank file format or fasta format, that contain the amino acid sequences of all the genes found in the BGC of interest. 

FunOrder performs a sequence similarity search using blastp on our manually curated database, multiple sequence alignment using the ClustalW algorithm, calculates the best scoring ML tree with RAxML (Randomized Axelerated Maximum Likelihood) for each gene and uses the TreeKO algorithm to calculate the pairwise distances between these trees. Based on these distances **FunOrder 2** automatically determines the optimal number of clusters in the output, and a subsequent k-means clustering based on the first three principal components of the PCAs clusters the genes/proteins into co-evolutionary linked protein families. See our newest publications for further details.

FunOrder 2 is provided with a database of ascomycete proteomes and can therefore be used for the detection of co-evolution of proteins in this fungal division. If other divisions, classes, or even kingdoms shall be analyzed, a suitable new proteome database must be compiled and tested, see our Wiki for further details.


Dependencies
------------

Third party programs

* [Python 2](https://www.anaconda.com)
* [Perl](https://www.perl.org/get.html)
* [R](https://www.r-project.org/)
* [Emboss](http://emboss.sourceforge.net/download/)
* [RAxML](https://github.com/stamatak/standard-RAxML)

Perl packages:

* [Bio::SeqIO](https://metacpan.org/pod/Bio::SeqIO)

Python packages:

* [ete2](https://pypi.org/project/ete2/)

R packages:

* readr
* stats
* gplots
* car
* mdatools
* xlsx
* cluster
* NbClust
* randtests


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
install.packages('readr') # at the R prompt
install.packages('stats') # at the R prompt
install.packages('gplots') # at the R prompt
install.packages('car') # at the R prompt
install.packages('mdatools') # at the R prompt
install.packages('xlsx') # at the R prompt
install.packages('cluster') # at the R prompt
install.packages('NbClust') # at the R prompt
install.packages('randtests') # at the R prompt
```

Now download FunOrder **funorder_XX.tar.xz** and unpack the archive.

```
tar -xf funorder_XX.tar.xz
```

open the scripts funorder.sh ; funorder_fasta_only.sh ; funorder_server.sh ; funorder_server_fasta_only.sh
change 'SOURCEDIR' value in line 43 in funorder.sh ; funorder_fasta_only.sh and line 45 in funorder_server.sh ; funorder_server_fasta_only.sh:

```
SOURCEDIR=~/funorder_proj/funorder_XX/ 
```
to (path to the funorder_XX directory: e.g. ~/path/to/your/directory/)
```
SOURCEDIR=~/path/to/your/directory/funorder_XX/
```

You can now add the FunOrder/pipeline directory to your $PATH environmental variable.
Alternativeley you can call the FunOrder/pipeline directory directly.




### 1) using FunOrder in default mode with Genbank files
------------

Run FunOrder from the folder containing the gbk file you want to analyze.
(cd ~/path/to/your/gbk_files)

```
sh ~/path/to/directory/funorder_XX/funorder.sh [Thread number] [gbk file] [absolute path to outputdirectory] [database]
```

or if you added the FunOrder/pipeline directory to your $PATH environmental variable.

```
sh funorder.sh [Thread number] [gbk file] [absolute path to outputdirectory] [database]
```

following folders will be created in the chosen outputdirectory:

1. file.gbk.analysis
2. file.gbk.analysis/alignment
3. file.gbk.analysis/hits

The output of the optional BLAST analysis is saved in the /file.gbk.analysis directory.
The output of FunOrder is saved in /file.gbk.analysis/alignment

#### Output files produced by funorder.sh

File                                | Description
------------------------------------|------------
FunOrder_Supplementary_Rplots.pdf   | PDF file with the Analyze.R output as described in our publication FunOrder 2
FunOrder_clustering_Rplots_pred.pdf | PDF file with the Analyze_clustering_pred.R output as described in our publication FunOrder 2
cluster_definition_pred.xlsx        | XLSX file with the Analyze_clustering_pred.R output as described in our publication FunOrder 2
strict_distance.matrix              | matrix of the strict distance
evol_distance.matrix                | matrix of the evolutionary [speciation] distance
Internal_coevolution_quotient.txt   | text file containing the ICQ analysis

if the automatic clustering failed then the outputfiles are

File                                   | Description
---------------------------------------|------------
FunOrder_Supplementary_Rplots.pdf      | PDF file with the Analyze.R output as described in our publication FunOrder 2
FunOrder_clustering_Rplots_defined.pdf | PDF file with the Analyze_clustering_defined.R output as described in our publication FunOrder 2
cluster_definition_3.xlsx              | XLSX file with the Analyze_clustering_defined.R output as described in our publication FunOrder 2
strict_distance.matrix                 | matrix of the strict distance
evol_distance.matrix                   | matrix of the evolutionary [speciation] distance
Internal_coevolution_quotient.txt      | text file containing the ICQ analysis



### 2) using FunOrder in default mode with fasta files
------------

Run FunOrder from the folder containing the fasta file you want to analyze.
(cd ~/path/to/your/fasta_files)

```
sh ~/path/to/directory/funorder_XX/funorder_fasta_only.sh [Thread number] [fasta file] [absolute path to outputdirectory] [database]
```

or if you added the FunOrder/pipeline directory to your $PATH environmental variable.

```
sh funorder_fasta_only.sh [Thread number] [fasta file] [absolute path to outputdirectory] [database]
```

following folders will be created in the chosen outputdirectory:

1. file.fasta.analysis
2. file.fasta.analysis/alignment
3. file.fasta.analysis/hits

The output of the optional BLAST analysis is saved in the /file.gbk.analysis directory.
The output of FunOrder is saved in /file.fasta.analysis/alignment

#### Output files produced by funorder_fasta_only.sh

File                                | Description
------------------------------------|------------
FunOrder_Supplementary_Rplots.pdf   | PDF file with the Analyze.R output as described in our publication FunOrder 2
FunOrder_clustering_Rplots_pred.pdf | PDF file with the Analyze_clustering_pred.R output as described in our publication FunOrder 2
cluster_definition_pred.xlsx        | XLSX file with the Analyze_clustering_pred.R output as described in our publication FunOrder 2
strict_distance.matrix              | matrix of the strict distance
evol_distance.matrix                | matrix of the evolutionary [speciation] distance
Internal_coevolution_quotient.txt   | text file containing the ICQ analysis

if the automatic clustering failed then the outputfiles are

File                                   | Description
---------------------------------------|------------
FunOrder_Supplementary_Rplots.pdf      | PDF file with the Analyze.R output as described in our publication FunOrder 2
FunOrder_clustering_Rplots_defined.pdf | PDF file with the Analyze_clustering_defined.R output as described in our publication FunOrder 2
cluster_definition_3.xlsx              | XLSX file with the Analyze_clustering_defined.R output as described in our publication FunOrder 2
strict_distance.matrix                 | matrix of the strict distance
evol_distance.matrix                   | matrix of the evolutionary [speciation] distance
Internal_coevolution_quotient.txt      | text file containing the ICQ analysis



### 3) using FunOrder in server mode with gbk files
------------

Run FunOrder from the folder containing the gbk file you want to analyze.
(cd ~/path/to/your/gbk_files)

```
sh ~/path/to/directory/funorder_XX/funorder_server.sh [Thread number] [gbk file] [absolute path to outputdirectory] [database]
```

or if you added the FunOrder/pipeline directory to your $PATH environmental variable.

```
sh funorder_server.sh [Thread number] [gbk file] [absolute path to outputdirectory] [database]
```

following folders will be created in the chosen outputdirectory:

1. file.gbk.analysis
2. file.gbk.analysis/alignment
3. file.gbk.analysis/hits

The output of FunOrder is saved in /file.gbk.analysis/alignment

#### Output files produced by funorder.sh

File                                | Description
------------------------------------|------------
FunOrder_Supplementary_Rplots.pdf   | PDF file with the Analyze.R output as described in our publication FunOrder 2
FunOrder_clustering_Rplots_pred.pdf | PDF file with the Analyze_clustering_pred.R output as described in our publication FunOrder 2
cluster_definition_pred.xlsx        | XLSX file with the Analyze_clustering_pred.R output as described in our publication FunOrder 2
strict_distance.matrix              | matrix of the strict distance
evol_distance.matrix                | matrix of the evolutionary [speciation] distance
Internal_coevolution_quotient.txt   | text file containing the ICQ analysis

if the automatic clustering failed then the outputfiles are

File                                   | Description
---------------------------------------|------------
FunOrder_Supplementary_Rplots.pdf      | PDF file with the Analyze.R output as described in our publication FunOrder 2
FunOrder_clustering_Rplots_defined.pdf | PDF file with the Analyze_clustering_defined.R output as described in our publication FunOrder 2
cluster_definition_3.xlsx              | XLSX file with the Analyze_clustering_defined.R output as described in our publication FunOrder 2
strict_distance.matrix                 | matrix of the strict distance
evol_distance.matrix                   | matrix of the evolutionary [speciation] distance
Internal_coevolution_quotient.txt      | text file containing the ICQ analysis



#### Example usage for generic antiSMASH output:

within the antiSMASH output-folder create a new directory "funorder_output"

```
mkdir funorder_output
```

then from within the antiSMASH output-folder run following command:

```
for file in *cluster*.gbk; do echo $file; sh ~/path/to/directory/funorder_XX/funorder_server.sh [Thread number] $file [absolute path to "funorder_output" directory] [database] ; done
```

This will perform a FunOrder analysis for each cluster predicted by antiSMASH.

### 4) using FunOrder in server mode with fasta files
------------

Run FunOrder from the folder containing the fasta file you want to analyze.
(cd ~/path/to/your/fasta_files)

```
sh ~/path/to/directory/funorder_XX/funorder_server_fasta_only.sh [Thread number] [fasta file] [absolute path to outputdirectory] [database]
```

or if you added the FunOrder/pipeline directory to your $PATH environmental variable.

```
sh funorder_server_fasta_only.sh [Thread number] [fasta file] [absolute path to outputdirectory] [database]
```

following folders will be created in the chosen outputdirectory:

1. file.fasta.analysis
2. file.fasta.analysis/alignment
3. file.fasta.analysis/hits

The output of FunOrder is saved in /file.fasta.analysis/alignment

#### Output files produced by funorder_fasta_only.sh

File                                | Description
------------------------------------|------------
FunOrder_Supplementary_Rplots.pdf   | PDF file with the Analyze.R output as described in our publication FunOrder 2
FunOrder_clustering_Rplots_pred.pdf | PDF file with the Analyze_clustering_pred.R output as described in our publication FunOrder 2
cluster_definition_pred.xlsx        | XLSX file with the Analyze_clustering_pred.R output as described in our publication FunOrder 2
strict_distance.matrix              | matrix of the strict distance
evol_distance.matrix                | matrix of the evolutionary [speciation] distance
Internal_coevolution_quotient.txt   | text file containing the ICQ analysis

if the automatic clustering failed then the outputfiles are

File                                   | Description
---------------------------------------|------------
FunOrder_Supplementary_Rplots.pdf      | PDF file with the Analyze.R output as described in our publication FunOrder 2
FunOrder_clustering_Rplots_defined.pdf | PDF file with the Analyze_clustering_defined.R output as described in our publication FunOrder 2
cluster_definition_3.xlsx              | XLSX file with the Analyze_clustering_defined.R output as described in our publication FunOrder 2
strict_distance.matrix                 | matrix of the strict distance
evol_distance.matrix                   | matrix of the evolutionary [speciation] distance
Internal_coevolution_quotient.txt      | text file containing the ICQ analysis


