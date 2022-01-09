# Genomes annotation

Here we show the codes used for genomes evaluation and genomes or proteomes annotations. 

### PROKKA genome annotation

We used prokka version 1.14.6. The complete code of this program is available at https://github.com/tseemann/prokka 

For each of the genomes of the analyses we ran the following base command:

`prokka --kingdom Bacteria --outdir "genome_annotation_output_folder" --genus Pseudomonas --centre X --cpus 0 --prefix "strain_name" genome_name.fasta`

This command will return a folder with many diverse annotation formats. We used the .gff files for the genomes evaluation and for the pangenome analyses, and the .faa files for some individual genomes annotation.

### Genomes evaluation

We use the QUAST program, including the BUSCO algorithm implemented in QUAST to analyze the quality and completeness of each of the genomes of our study. The complete manual is avaiable at http://quast.sourceforge.net/docs/manual.html 

Here we used the following command, which was run on a folder containing all the .gff files from prokka annotations:

quast.py -o output_quast_busco -t 6 -b *.gff

### CAZys annotation

(incluir el filtro de 2/3)

### MEROPS annotation

### HGTector commands

### AMRFinder commands
