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

`quast.py -o output_quast_busco -t 6 -b *.gff`

### CAZys annotation

To annotate the proteomes against the CAZy database we sued the dbCAN2 program implemented within the run_dbcan.py scritp, available at: https://github.com/linnabrown/run_dbcan 

We ran: `run_dbcan.py "proteomes".faa protein --out_dir output_dbcan --dia_cpu 10 --hmm_cpu 10 --hotpep_cpu 10 --tf_cpu 10 --tf_cpu 10 --db_dir /home/zaki/db`

We then filtered te results to retain only those proteins annotated by at least 2 of the 3 algorithms included in the script.

`for file in *.txt ;
do ;
echo "awk '$5~/[23]/ {print $0}' ${file} > 2-3-tools-dbcan_${file}" ;
done > sort_dbcan_output.sh`

Then:
`bash sort_dbcan_output.sh`


### MEROPS annotation

We used the **DIAMOND** algorithm (https://github.com/bbuchfink/diamond) to annotate our proteomes against the **MEROPS database** (https://www.ebi.ac.uk/merops/).

First, we downloaded the database from here: https://www.ebi.ac.uk/merops/download_list.shtml. Concretely, from the "Peptidase Protein Sequences" section

Then, we formated the database as follows:

'awk '{print $1}' pepunit.lib > pepunit.faa`
'diamond makedb --in pepunit.faa -d merops`

FInally, to run de diamond search we ran:
`diamond blastp --db merops.dmnd -q "proteomes".faa -o merops_peptidases`


### SignalP searches

### HGTector commands

### AMRFinder commands
