# Pangenome analyses

To create clusters of proteins and facilitate the genomic comparisons and pangenome calculations we used the PPanGGOLiN program (https://github.com/labgem/PPanGGOLiN). This program enables to create useful tables, graphics and statistics. 

First, we formatted the already annotated genomes (gff file outputs from prokka, see [annotation](./genomes_annotation.md)). To do so, we will need a list of  our genome names and genome files that we have in the current directory (see this [example](https://github.com/labgem/PPanGGOLiN/edit/master/testingDataset/organisms.gbff.list)). Now, we can use the first PPanGGOLiN command:

~~~
$ ppanggolin annotate --anno pseudomonas_annotation_list.txt --cpu 6
~~~

This previous step creates a file called "pangenome.h5" that will store many data of our pangenome generated with PPanGGOLiN. Then, we ran a command to cluster the proteins with [MMSeqs2](https://github.com/soedinglab/MMseqs2)

~~~
$ ppanggolin cluster -p pangenome.h5 --identity 0.7
~~~

Now we will create a presence-absence matrix of protein clusters. This is a huge table that provides information on wheter a genome has some protein falling in a certain protein cluster or not.  

~~~
$ ppanggolin write -p pangenome.h5 --csv --output matrix_ppanggolin
~~~

Within the matrix_ppanggolin folder we will find a **matrix.csv** table that will be used with **Scoary** as detailed below, in the next section.

Now, the following subcommand will build a said pangenome graph, which will take a pangenome .h5 file as input and add edges to it. 

~~~
$ ppanggolin graph -p pangenome.h5
~~~

The `partition` subcommand will assign protein families to the 'persistent', 'shell', or 'cloud' partitions.

~~~
$ ppanggolin partition -p pangenome.h5 -c 10
~~~

To obtain basics stats from the pangenome built with the previous commands, we just ran:

~~~
$ ppanggolin write -p pangenome.h5 --stats
~~~

We generated an rarefaction plot representing the evolution of the number of gene families for each partition as you add more genomes to the pangenome. This is a .html file that can be opened with any browser.

~~~
$ ppanggolin rarefaction -p pangenome.h5 -c 10
~~~

Some of our analyses required the representative sequences for each protein cluster. To obtain them, we used this command:

~~~
$ ppanggolin fasta -p pangenome.h5 --output representative_sequences --prot_families all
~~~

#### To find Regions of Genome Plasticity (RGPs)
~~~
 $ ppanggolin panrgp --anno 3301_pseudomonas_annotation_list.txt -c 10
 ~~~
 
 # Pangenome wide association studies
 
 To search for significant associations of proteins and/or functions to the different isolation sources of the 3,274 _Pseudomonas_ strains of this work, we used [Scoary](https://github.com/AdmiralenOla/Scoary). This program requires two input files:
 - A matrix in which each row represents a feature (which in our case are proteins or functions) and each column represents a genome. Then, the cells shows the presence ("1" or any text) or abscence ("0") of each feature in each genome. We used the `matrix.csv` file created with PPanGGOLiN for protein analyses and simmilar formatted files for CAZy, resistance-genes and COGs analyses.
 - A traits table as [detailed](https://github.com/AdmiralenOla/Scoary) by the developers 
