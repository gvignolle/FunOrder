FunOrder
=========

The Functional Order (FunOrder) tool - Identification of essential biosynthetic genes through computational molecular co-evolution – searches for co-evolutionary linked genes in a set of inputted genes. The functionality and applicability was tested with biosynthetic gene clusters (BGCs). The resulting information can be used to choose which genes of a gene cluster are most likely the core genes necessary for the biosynthesis of a secondary metabolite. The flexibility and adaptability of the core program allows the integration of any protein database and can thus be adapted for different phyla and research objectives. FunOrder might be used for the analysis of co-evolution on a whole proteome, enabling the genome wide detection of evolutionary linked genes, in the future. 

The Functional Order (FunOrder) tool - Identification of essential biosynthetic genes through computational molecular co-evolution. FunOrder is copyright 2020 Gabriel A. Vignolle, Denise Schaffer, Robert L. Mach, Astrid R. Mach-Aigner and Christian Derntl, and is released under the MIT License. If you find FunOrder useful to your work, please cite:

https://doi.org/10.5281/zenodo.4778487 for the code or 

**The Functional Order (FunOrder) tool - Identification of essential biosynthetic genes through computational molecular co-evolution** Gabriel A Vignolle, Denise Schaffer, Robert L Mach, Astrid R Mach-Aigner, Christian Derntl. **bioRxiv** 2021.01.29.428829; doi: https://doi.org/10.1101/2021.01.29.428829

The software input files are biosynthetic gene clusters (BGC) with gene translations in genbank file format or fasta format, that contain the amino acid sequences of all the genes found in the BGC of interest. 

FunOrder performs a sequence similarity search using blastp on our manually curated database, multiple sequence alignment using the ClustalW algorithm, calculates the best scoring ML tree with RAxML (Randomized Axelerated Maximum Likelihood) for each gene and uses the TreeKO algorithm to calculate the pairwise distances between these trees. All pairwise **strict** and **evolutionary** distances are saved as matrices respectively. The matrices are used as input for an R script for visualization and further analysis of the distances. The strict and evolutionary distances are summed up to a third **combined** distance measure. For further detail and an exemplary analysis of the FunOrder output, see our publication.

The three distance matrices are first visualized as heatmaps with a dendrogram computed with the complete linkage method, that finds similar clusters. Then the Euclidean distance within the matrices is computed and clustered using Ward’s minimum variance method aiming at finding compact spherical clusters, with the implemented squaring of the dissimilarities before cluster updating, for each of the three distance matrices separately, with scaled and unscaled input data. Lastly a principle component analysis (PCA) is performed on each distance matrix and the score plot of the first two principle components visualized, respectively. FunOrder includes scripts adapted to the use on servers and for the integration in various pipelines.


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
```

Now download FunOrder **funorder_v1.tar.xz** and unpack the archive.

```
tar -xf funorder_v1.tar.xz
```

open the scripts funorder.sh ; funorder_fasta_only.sh ; funorder_server.sh ; funorder_server_fasta_only.sh
change 'SOURCEDIR' value in line 43 in funorder.sh ; funorder_fasta_only.sh and line 45 in funorder_server.sh ; funorder_server_fasta_only.sh:

```
SOURCEDIR=~/funorder_proj/funorder_v1/ 
```
to (path to the funorder_v1 directory: e.g. ~/path/to/your/directory/)
```
SOURCEDIR=~/path/to/your/directory/funorder_v1/
```

You can now add the FunOrder/pipeline directory to your $PATH environmental variable.
Alternativeley you can call the FunOrder/pipeline directory directly.



### 1) using FunOrder in default mode with Genbank files
------------

Run FunOrder from the folder containing the gbk file you want to analyze.
(cd ~/path/to/your/gbk_files)

```
sh ~/path/to/directory/funorder_v1/funorder.sh [Thread number] [gbk file] [absolute path to outputdirectory] [database]
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

File                         | Description
-----------------------------|------------
Rplot.pdf                    | PDF file with the Analyze.R output as described in our publication
strict_distance.matrix       | matrix of the strict distance
evol_distance.matrix         | matrix of the evolutionary [speciation] distance



### 2) using FunOrder in default mode with fasta files
------------

Run FunOrder from the folder containing the fasta file you want to analyze.
(cd ~/path/to/your/fasta_files)

```
sh ~/path/to/directory/funorder_v1/funorder_fasta_only.sh [Thread number] [fasta file] [absolute path to outputdirectory] [database]
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

File                         | Description
-----------------------------|------------
Rplot.pdf                    | PDF file with the Analyze.R output as described in our publication
strict_distance.matrix       | matrix of the strict distance
evol_distance.matrix         | matrix of the evolutionary [speciation] distance



### 3) using FunOrder in server mode with gbk files
------------

Run FunOrder from the folder containing the gbk file you want to analyze.
(cd ~/path/to/your/gbk_files)

```
sh ~/path/to/directory/funorder_v1/funorder_server.sh [Thread number] [gbk file] [absolute path to outputdirectory] [database]
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

File                         | Description
-----------------------------|------------
Rplot.pdf                    | PDF file with the Analyze.R output as described in our publication
strict_distance.matrix       | matrix of the strict distance
evol_distance.matrix         | matrix of the evolutionary [speciation] distance


#### Example usage for generic antiSMASH output:

within the antiSMASH output-folder create a new directory "funorder_output"

```
mkdir funorder_output
```

then from within the antiSMASH output-folder run following command:

```
for file in *cluster*.gbk; do echo $file; sh ~/path/to/directory/funorder_v1/funorder_server.sh [Thread number] $file [absolute path to "funorder_output" directory] [database] ; done
```
This will perform a FunOrder analysis for each cluster predicted by antiSMASH.



### 4) using FunOrder in server mode with fasta files
------------

Run FunOrder from the folder containing the fasta file you want to analyze.
(cd ~/path/to/your/fasta_files)

```
sh ~/path/to/directory/funorder_v1/funorder_server_fasta_only.sh [Thread number] [fasta file] [absolute path to outputdirectory] [database]
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

File                         | Description
-----------------------------|------------
Rplot.pdf                    | PDF file with the Analyze.R output as described in our publication
strict_distance.matrix       | matrix of the strict distance
evol_distance.matrix         | matrix of the evolutionary [speciation] distance


