# Pangenome analyses

To create clusters of proteins and facilitate the genomic comparisons and pangenome calculations we used the PPanGGOLiN program (https://github.com/labgem/PPanGGOLiN). This program enables to create useful tables, graphics and statistics. 

First, we formatted the already annotated genomes (gff file outputs from prokka, see [annotation](./genomes_annotation.md))
ppanggolin annotate --anno 3301_pseudomonas_annotation_list.txt --cpu 6
ppanggolin cluster -p pangenome.h5 --identity 0.7
ppanggolin write -p pangenome.h5 --csv --output matrix_ppangolin_3301

Continuación:
ppanggolin graph -p pangenome.h5
ppanggolin partition -p pangenome.h5 -c 10
ppanggolin write -p pangenome.h5 --stats

ppanggolin write -p pangenome.h5 --families_tsv
ppanggolin write -p pangenome.h5 --Rtab   Con esto podré hacer curvas después


ppanggolin draw -p pangenome.h5 --ucurve

ppanggolin draw -p pangenome.h5 --tile_plot --nocloud -c 10

ppanggolin rarefaction -p pangenome.h5 -c 10

 ppanggolin panrgp --anno 3301_pseudomonas_annotation_list.txt -c 10
 
 # Pangenome wide association studies
 
 
