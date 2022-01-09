# Pangenome analyses

To create clusters of proteins and facilitate the genomic comparisons and pangenome calculations we used the PPanGGOLiN program (https://github.com/labgem/PPanGGOLiN). This program enables to create useful tables, graphics and statistics. 

First, we formatted the already annotated genomes (gff file outputs from prokka, see [annotation](./genomes_annotation.md)). To do so, we will need a list of  our genome names and genome files that we have in the current directory (see this [example](https://github.com/labgem/PPanGGOLiN/edit/master/testingDataset/organisms.gbff.list)). Now, we can use the first PPanGGOLiN command:

`$ ppanggolin annotate --anno pseudomonas_annotation_list.txt --cpu 6`

This previous step creates a file called "pangenome.h5" that will store many data of our pangenome generated with PPanGGOLiN. Then, we ran a command to cluster the proteins with [MMSeqs2](https://github.com/soedinglab/MMseqs2)

`$ ppanggolin cluster -p pangenome.h5 --identity 0.7`

Now we will create a presence-absence matrix of protein clusters. This is a huge table that provides information on wheter a genome has some protein falling in a certain protein cluster or not.  

`$ ppanggolin write -p pangenome.h5 --csv --output matrix_ppanggolin`

Within the matrix_ppanggolin folder we will find a matrix.csv table that will be used with Scoary as detailed below, in the next section.

Now, the following subcommand will build a said pangenome graph, which will take a pangenome .h5 file as input and add edges to it. 

`$ ppanggolin graph -p pangenome.h5`
ppanggolin partition -p pangenome.h5 -c 10
ppanggolin write -p pangenome.h5 --stats

ppanggolin write -p pangenome.h5 --families_tsv
ppanggolin write -p pangenome.h5 --Rtab   Con esto podré hacer curvas después


ppanggolin draw -p pangenome.h5 --ucurve

ppanggolin draw -p pangenome.h5 --tile_plot --nocloud -c 10

ppanggolin rarefaction -p pangenome.h5 -c 10

 ppanggolin panrgp --anno 3301_pseudomonas_annotation_list.txt -c 10
 
 # Pangenome wide association studies
 
 
