---
layout: post
title: Random Forest in R
---

## What is Random Forest

Tree model is a famous class in machine learning. Most people have heard about decision tree. Random forest is also a tree based classification/regression algorithm, however, instead of constructing one tree, it constructs many trees and the result that has the most votes wins[^1]. Random Forest is considered one of the most useful models as it is appliable to most scenarios. When you encounter a new problem and have no clue what to use, try random forest first. 

## Random Forest in R

In *R*, we use `randomForest` package to do random forest tasks.
``` r
#install.packages("randomForest") # if not installed
require(randomForest)
```

Assume we have a dataset name `data1`, which contains many rows and several columns (let's assume these columns named `y`, `x1`, `x2`, etc. where `y` is a factor variable for classification; you can try some real datasets such as the famous `iris` dataset). We can build a random forest model as following,
```r
rf.model <- randomForest(y~.,
    data=data1,
    keep.forest=TRUE,
    importance=TRUE,
    ntree=200,
    mtry=3)
rf.model # print out the model summary
```

Note `importance` argument let us observe the improtance of features.
```r
imp <- importance(x=rf.model)
imp
```

Let's say that `data2` is our test data, we can apply the random forest model to the data.
```r
pred <- predict(rf.model, data=data2)
freq <- table(pred, data2$y)
sum(diag(freq))/sum(freq) # report accuracy
plot(rf.model) # plot the error rate vs number of trees
```

### Interpret varImpPlot

If we use function `varImpPlot()` on a random forest model, it will give us two plots, namely, mean decrease in accuracy and mean decrease in gini. The former one is larger for more important variables, whereas the latter one is larger for variables that can bring more purity during random forest classification.

So the plots are useful for user to choose over features used for random forest.

## How to improve its performance

In order to improve random forest performance, the first question one shall ask is that which parameters affect the performance? According to this post[^2], following parameters are particular important:
	* `mtry`: the number of variables randomly sampled as candidates at each split; less is better
	* `ntrees`
	* number of features: the number of columns in the dataset; the more the slower
	* number of observations: the number of rows in the dataset
	* `ncores`
	* set `importance=FALSE` and `proximity=FALSE`
	* Never use `nodesize=1`; instead, say we want each node contain 0.01% data, for 4.2e5 rows, we should set `nodesize=42`
	* `strata` and `sampsize` could improve the speed
	
To improve the speed, feature selection is necessary.

To improve the performance, `mtry` and `ntrees` are two important parameters to tune. `mtry` could be optimized using `tuneRF()` function in the `randomForest` package. `ntrees` vs error rates can be visualized by `plot(random.forest.model)` where `random.forest.model` is the variable name for the random forest object.

## Conclusion

Random forest is robust and one of the most favorite algorithms to do regression or classification. Using `randomForest` package, implementing and tuning random forest is simple in R.

--------
[^1]: Leo Breiman and Adele Cutler. Random Forests. https://www.stat.berkeley.edu/~breiman/RandomForests/cc_home.htm
[^2]: smci. StackOverflow. https://stackoverflow.com/questions/23075506/how-to-improve-randomforest-performance
[^3]: R语言之Random Forest随机森林. https://www.cnblogs.com/nxld/p/6374945.html
[^4]: Anish Singh Walia. Random Forests in R. https://datascienceplus.com/random-forests-in-r/


<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>