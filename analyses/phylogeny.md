# *Pseudomonas* phylogeny

To delete extremely distant genomes and to study the phylogeny of the 3,274 selected _Pseudomonas_ genomes, we used the **UBCG tool** (v3.0), which can be downloaded [here](https://www.ezbiocloud.net/tools/ubcg)

The first command will be used to convert each unnanotated genome into UBCG format (.bcg extension). We will use this command within a directory containing all the genomes in fasta format.

~~~
$ for file in *.fna ; do ; genome={file%%.fna} echo "java -jar UBCG.jar extract -bcg_dir bcg_pangenome_pseudomonas -i ${file} -label "${genome}"
~~~

Then, we will have a folder called "bcg_pangenome_pseudomonas" containing a sigle .bcg file for each of our genomes. These files will contain UBCG sequences for further analyses. Now we are ready to build the phylogenies:

~~~
$ java -jar UBCG.jar align -bcg_dir bcg_pangenome_pseudomonas/  -prefix pangenome_pseudomonas
~~~

Now we will look for the output file output/pangenome_pseudomonas/pangenome_pseudomonas.UBCG_gsi(92).codon.50.label.nwk , which we uploaded into the [iTOL](https://itol.embl.de/) program  to add metadata and display a rerendered phylogenetic tree.
