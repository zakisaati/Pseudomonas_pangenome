# Genomes annotation

Here we show the codes used for genomes evaluation and genomes or proteomes annotations. 


## PROKKA genome annotation

We used **prokka** version 1.14.6. The complete code of this program is available [here](https://github.com/tseemann/prokka)

For each of the genomes of the analyses we ran the following base command:

~~~
$ prokka --kingdom Bacteria --outdir "genome_annotation_output_folder" --genus Pseudomonas --centre X --cpus 0 --prefix "strain_name" genome_name.fasta
~~~

This command will return a folder with many diverse annotation formats. We used the .gff files for the genomes evaluation and for the pangenome analyses, and the .faa files for some individual genomes annotation.


## Genomes evaluation

We use the **QUAST** program, including the **BUSCO** algorithm implemented in QUAST to analyze the quality and completeness of each of the genomes of our study. The complete manual is available under this [link](http://quast.sourceforge.net/docs/manual.html)

Here we used the following command, which was run on a folder containing all the .gff files from prokka annotations:

~~~
$ quast.py -o output_quast_busco -t 6 -b *.gff
~~~


## CAZys annotation

To annotate the proteomes against the **CAZy database** we sued the **dbCAN2** program implemented within the [run_dbcan.py](https://github.com/linnabrown/run_dbcan) script 

We ran: 
~~~
$ run_dbcan.py "proteomes".faa protein --out_dir output_dbcan --dia_cpu 10 --hmm_cpu 10 --hotpep_cpu 10 --tf_cpu 10 --tf_cpu 10 --db_dir /home/zaki/db
~~~

We then filtered the results to retain only those proteins annotated by at least 2 of the 3 algorithms included in the script.

~~~
$ for file in *.txt 
do 
echo "awk '$5~/[23]/ {print $0}' ${file} > 2-3-tools-dbcan_${file}" 
done > sort_dbcan_output.sh
~~~

Then:
~~~
$ bash sort_dbcan_output.sh
~~~


## MEROPS annotation

We used the **DIAMOND** algorithm (https://github.com/bbuchfink/diamond) to annotate our proteomes against the **MEROPS database** (https://www.ebi.ac.uk/merops/).

First, we downloaded the database from here: https://www.ebi.ac.uk/merops/download_list.shtml. Concretely, from the "Peptidase Protein Sequences" section

Then, we formatted the database as follows:

~~~
$ awk '{print $1}' pepunit.lib > pepunit.faa
~~~

~~~
$ diamond makedb --in pepunit.faa -d merops
~~~


Finally, to run de diamond search we ran:
~~~
$ diamond blastp --db merops.dmnd -q "proteomes".faa -o merops_peptidases
~~~


## Signal peptide searches

To look for proteins with a signal peptide we used the **SignalP** tool. We downloaded it from: https://services.healthtech.dtu.dk/cgi-bin/sw_request

The command used was:

~~~
$ signalp -fasta sequences.faa -org gram- -format short -prefix signalp_output.faa
~~~


## HGTector2 commands

To look for potential genes that have been horizontally transferred, we used the HGTector2 program following the instructions available [here](https://github.com/qiyunlab/HGTectorh)

First we compiled a microbes database as detailed in https://github.com/qiyunlab/HGTector/blob/master/doc/database.md. 

Secondly, we searched for putative horizontal gene transfer (HGT) events with the following command:

~~~
$ hgtector search -i pangenome_proteins.faa -o search_pangenome_proteins -m diamond -p 0 -d hgtdb/diamond/db -t hgtdb/taxdump --evalue 1e-10 --tax-unirank species
~~~

Finally, we create analyses of the previous report:
~~~
$ hgtector analyze -i search_pangenome_proteins -o out_analyze_pangenome_proteins -t hgtdb/taxdump --donor-name
~~~

Then, we inspected the results and mannually filtered them

## AMRFinder commands

To look for proteins involved in antimicrobial, biocide or stresses resistances we annotate the proteomes with the [AMRFinder_plus](https://github.com/ncbi/amr) program 

The command that we used is:

~~~
$ amrfinder -p protein_sequences.faa -o output_amrfinder --plus
~~~

---
