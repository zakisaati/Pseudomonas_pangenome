


## PCA plot

~~~~
 library("ggplot2")
 library(factoextra)
 test <- read.csv("matrix_COGs_3274.csv", row.names=1, check.names=FALSE)
 input <- as.data.frame(test)[1:2164]
 res.pca <- prcomp(input, scale = FALSE)
 fviz_eig(res.pca)
fviz_pca_ind(res.pca)
 fviz_pca_ind(res.pca, label="none")
 fviz_pca_ind(res.pca, label="none", habillage=test$Niche)
~~~~~
