# *Pseudomonas* phylogeny

To delete extremely distant genomes and to study the phylogeny of the 3,274 selected _Pseudomonas_ genomes, we used the **UBCG tool** (v3.0), which can be downloaded [here](https://www.ezbiocloud.net/tools/ubcg)

The first command will be used to convert each unannotated genome into UBCG format (.bcg extension). We will use this command within a directory containing all the genomes in fasta format.

~~~
$ for file in *.fna
do
genome={file%%.fna}
echo "java -jar UBCG.jar extract -bcg_dir bcg_pangenome_pseudomonas -i ${file} -label "${genome}"
done > extract_commands.sh
~~~

Now, we execute the commands:
~~~
$ bash extract_commands.sh
~~~
Then, we will have a folder called "bcg_pangenome_pseudomonas" containing a single .bcg file for each of our genomes. These files will contain UBCG sequences for further analyses. Now we are ready to build the phylogenies:

~~~
$ java -jar UBCG.jar align -bcg_dir bcg_pangenome_pseudomonas/  -prefix pangenome_pseudomonas
~~~

Now we will look for the output file output/pangenome_pseudomonas/pangenome_pseudomonas.UBCG_gsi(92).codon.50.label.nwk , which we uploaded into the [iTOL](https://itol.embl.de/) program  to add metadata and display a rerendered phylogenetic tree.

We removed those genomes that are extremely distant to the others in the pangenome. As an example, see the following phylogenetic tree:

<img width="908" alt="Captura de pantalla 2022-09-15 a las 9 51 09" src="https://user-images.githubusercontent.com/50806485/190347268-e0cf1769-7cda-4323-9a45-df3e52adc3ec.png">

Then, the phylogenetic tree including the final 3,274 genomes, after adding metadata and rerendering it in iTOL is:

<img width="861" alt="Captura de pantalla 2022-09-15 a las 9 53 30" src="https://user-images.githubusercontent.com/50806485/190347798-8aa74a07-da93-43af-8fec-1aa6ec83e3ec.png">


---
## GTDB-Tk taxonomy
To study the taxonomic placement of each strain we ran the [GTDB-Tk](https://ecogenomics.github.io/GTDBTk/announcements.html) program through its ["classify_wf"](https://ecogenomics.github.io/GTDBTk/commands/classify_wf.html) command. 

~~~
$ gtdbtk classify_wf --genome_dir 3274_genomes/ --out_dir gtdbtk_pseudomonas -x fna --cpus 6 --scratch_dir scratch
~~~

Due to RAM issues, we needed to split the analyses into several runs. Then, we merged the output files into a single file. From that, we used the "classification" column to assign the taxonomy to each genome.
