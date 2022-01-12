


## PCA

We created a PCA based on the presence/absence of COG functions in each genome and we colored each genome based on the isolation source. For this, we need a [csv matrix](./Source_data/PCA/matrix_COGs_3274.csv) as input. We used R to create this graph:

~~~~
> library("ggplot2")
> library(factoextra)
> test <- read.csv("matrix_COGs_3274.csv", row.names=1, check.names=FALSE)
> input <- as.data.frame(test)[1:2164]
> res.pca <- prcomp(input, scale = FALSE)
> fviz_eig(res.pca)
> fviz_pca_ind(res.pca, label="none", habillage=test$Niche)
~~~~~

## Boxplots

We created several boxplots to compare the abundance of different features in each of the 3,274 _Pseudomonas_ strains. All the input tables are available in the
["input tables for boxplots"](./Source_data/Boxplots/) folder. The R code is:

~~~~~
> library(ggplot2)
> options(scipen=999)
> data <- read.csv("input_table.csv")
> p <- ggplot(data, aes(x=Category, y=Genes)) + geom_boxplot() + theme_classic()
> p
~~~~~

## Density plots 

Similarly, we created density plots to study the distribution of COG numbers in each category of genomes. The input data are available in [this folder](./Source_data/Density_plots/)

~~~~~
> library(ggplot2)
> data <- read.csv("input_table.csv")
> p <- ggplot(data, aes(x=GC)) + geom_density(color= "pale green", fill="palegreen") + theme_minimal()
> p
~~~~~


## UpSetR

To compare the content of COGs among all the categories we used [UpSetR](https://github.com/hms-dbmi/UpSetR), which creates graphics with similar meaning than a Venn Diagram, but in a more explicative way. The input table is located [here](./Source_data/UpSetR/input_upsetr_1_0.csv). The code is as follows:

~~~~~
> library(UpSetR)
> test <- read.csv("input_upsetr_1_0.csv", header=T, check.names=FALSE)
> upset(test, sets = c("Host-associated", "Human", "Algae", "Aves", "Fish", "Mamalia", "Nematoda", "Plants"))
> upset(test_valores_encab, nsets= 15, order.by = "freq")
~~~~~

## Heatmap in R

To create heatmaps of COG categories, we used this [input matrices](./Source_data/Heatmap_COG_categories/) and ran the following commands:

~~~~~
> library("gplots")
> library("heatmap.plus")
> library("RColorBrewer")
> test <- read.csv("Matrix_heatmap_COGs_enriched_relative_abundance.csv",row.names = 1, check.names=FALSE)
> input <- as.matrix(test)
> my_palette <- colorRampPalette(c("white", "blue", "darkblue"))(n = 1000)
> heatmap.2(input, trace="none", density="none", col=my_palette, cexRow=0.6, cexCol=0.6, margins = c(17,25), scale="none")
~~~~~

## Heatmap of the pangenome with python

To create a big heatmap of the pangenome COG content, we used the [matrix of COG presence/absence](./Source_data/Heatmap_pangenome_cogs/matrix_COGs_presence_absence.csv) and the pangenome [phylogenetic tree](./Source_data/Heatmap_pangenome_cogs/phylogenetic_tree_UBCG_pangenome_pseudomonas_3274.nwk). The script to create it was already public: [roary_plots.py](https://github.com/sanger-pathogens/Roary/tree/master/contrib/roary_plots)

~~~~~
$ roary_plots.py UBCG_tree.nwk Matrix_COGs_presence_absence.csv
~~~~~
