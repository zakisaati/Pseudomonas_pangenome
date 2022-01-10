


## PCA

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

~~~~~
> library(ggplot2)
> options(scipen=999)
> data <- read.csv("input_table.csv")
> p <- ggplot(data, aes(x=Category, y=Genes)) + geom_boxplot() + theme_classic()
> p
~~~~~


## Density plots 

~~~~~
> library(ggplot2)
> data <- read.csv("input_table.csv")
> p <- ggplot(data, aes(x=GC)) + geom_density(color= "pale green", fill="palegreen") + theme_minimal()
> p
~~~~~


## UpSetR

~~~~~
> library(UpSetR)
> test <- read.csv("input_upsetr_1_0.csv", header=T, check.names=FALSE)
> upset(test, sets = c("Host-associated", "Human", "Algae", "Aves", "Fish", "Mamalia", "Nematoda", "Plants"))
> upset(test_valores_encab, nsets= 15, order.by = "freq")
~~~~~

## Heatmap in R

~~~~~
> library("gplots")
> library("heatmap.plus")
> library("RColorBrewer")
> test <- read.csv("matrix_heatmap_enriched_COGs.csv",row.names = 1, check.names=FALSE)
> input <- as.matrix(test)
> my_palette <- colorRampPalette(c("white", "blue", "darkblue"))(n = 1000)
> heatmap.2(input, trace="none", density="none", col=my_palette, cexRow=0.6, cexCol=0.6, margins = c(17,25), scale="none")
~~~~~

## Heatmap of the pangenome with python

~~~~~
$ roary_plots.py UBCG_tree.nwk Matrix_COGs_presence_absence.csv
~~~~~
