---
layout: post
title: Random Forest in R
---

## What is Random Forest

Tree is a structure to classify things. The most well known tree is decision tree. Random forest is also a tree based classification/regression algorithm, however, instead of constructing one tree, it constructs many trees and the tree has most votes wins. 

## Random Forest in R

In *R*, we use `randomForest` package to do random forest tasks.
``` r
#install.packages("randomForest") # if not installed
require(randomForest)
```

### Interpret varImpPlot

If we use function `varImpPlot()` on a random forest model, it will give us two plots, namely, mean decrease in accuracy and mean decrease in gini. The former one is larger for more important variables, whereas the latter one is larger for variables that can bring more purity during random forest classification.

So the plots are useful for user to choose over features used for random forest.

## How to improve performance

In order to improve random forest performance, the first question one shall ask is that which parameters affect the performance? According to [the post by smci](https://stackoverflow.com/questions/23075506/how-to-improve-randomforest-performance), following parameters are particular important:
	+ `mtry`: the number of variables randomly sampled as candidates at each split; less is better
	+ `ntrees`
	+ number of features: the number of columns in the dataset; the more the slower
	+ number of observations: the number of rows in the dataset
	+ `ncores`
	+ set `importance=FALSE` and `proximity=FALSE`
	+ Never use `nodesize=1`; instead, say we want each node contain 0.01% data, for 4.2e5 rows, we should set `nodesize=42`
	+ `strata` and `sampsize` could improve the speed
	
To improve the speed, feature selection is necessary.

To improve the performance, `mtry` and `ntrees` are two important parameters to tune. `mtry` could be optimized using `tuneRF()` function in the `randomForest` package. `ntrees` vs error rates can be visualized by `plot(random.forest.model)` where `random.forest.model` is the variable name for the random forest object.
