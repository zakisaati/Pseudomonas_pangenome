# Genomes annotation

Here we show the codes used for genomes evaluation and genomes or proteomes annotations. 

## PROKKA genome annotation

We used prokka version 1.14.6. The complete code of this program is available at https://github.com/tseemann/prokka 

For each of the genomes of the analyses we ran the following base command:

`prokka --kingdom Bacteria --outdir "genome_annotation_output_folder" --genus Pseudomonas --centre X --cpus 0 --prefix "strain_name" genome_name.fasta`



**bold text**
