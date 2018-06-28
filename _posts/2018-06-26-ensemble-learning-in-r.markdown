---
layout: post
title: Ensemble Learning in R
---

## What is ensemble learning

In practice, machine learning algorithms do not perform as good as we thought. In this case, besides tuning the algorithms, another solution is to combine multiple learners derived by different techniques and create a stronger one. This is so called **ensemble learning**[^1].

The idea behind ensemble learning is simple, but the technique requires careful design and tuning. In many cases, the ensemble learner created is no better than individual model. However, playing around with different sets of algorithms and their parameters could lead you to a better overall model[^2]. This is the spirit of ensemble learning.

## Ensemble learning in R

In this section, we will explain how to conduct ensemble learning in *R*. Before that, we need to introduce a package named *SuperLearner*, which makes ensemble learning in *R* much easier.

### SuperLearner

This *R* package provides an easier way to create ensemble by using wrappers of different popular machine learning libraries such as `knn`, `kernlab`, `randomForest` etc. A detailed guide of *SuperLearner* can be found [here](https://cran.r-project.org/web/packages/SuperLearner/vignettes/Guide-to-SuperLearner.html).

Here, we will explain some basic usage. To use *SuperLearner*, we can install the package and load it.

```r
install.packages("SuperLearner")
require(SuperLearner)
```

As we mentioned before, *SuperLearner* provides wrappers for different machine learning libraries, therefore, to use these machine learning algorithms, we need to install them separately.

```r
install.packages(c("caret", "glmnet", "kernlab", "randomForest"))
require(caret)
require(glmnet)
require(kernlab)
require(randomForest)
```

We can review available models by 

```r
listWrappers()
```

```
## All prediction algorithm wrappers in SuperLearner:

##  [1] "SL.bartMachine"      "SL.bayesglm"         "SL.biglasso"        
##  [4] "SL.caret"            "SL.caret.rpart"      "SL.cforest"         
##  [7] "SL.dbarts"           "SL.earth"            "SL.extraTrees"      
## [10] "SL.gam"              "SL.gbm"              "SL.glm"             
## [13] "SL.glm.interaction"  "SL.glmnet"           "SL.ipredbagg"       
## [16] "SL.kernelKnn"        "SL.knn"              "SL.ksvm"            
## [19] "SL.lda"              "SL.leekasso"         "SL.lm"              
## [22] "SL.loess"            "SL.logreg"           "SL.mean"            
## [25] "SL.nnet"             "SL.nnls"             "SL.polymars"        
## [28] "SL.qda"              "SL.randomForest"     "SL.ranger"          
## [31] "SL.ridge"            "SL.rpart"            "SL.rpartPrune"      
## [34] "SL.speedglm"         "SL.speedlm"          "SL.step"            
## [37] "SL.step.forward"     "SL.step.interaction" "SL.stepAIC"         
## [40] "SL.svm"              "SL.template"         "SL.xgboost"

## 
## All screening algorithm wrappers in SuperLearner:

## [1] "All"
## [1] "screen.SIS"            "screen.corP"           "screen.corRank"       
## [4] "screen.glmnet"         "screen.randomForest"   "screen.template"      
## [7] "screen.ttest"          "write.screen.template"
```


### Build an ensemble

Assume we have a training dataset name `data1`, which contains many rows and several columns (let's assume these columns named `y`, `x1`, `x2`, etc. where `y` is a factor variable for classification; you can try some real datasets such as the famous `iris` dataset). Assume the test dataset name `data2`. We can build an ensemble model (for now, let's create a SVM + randomForest model) as following,

```r
# encode response variable for SuperLearner
# SuperLearner requires factor variable to be converted to int
y <- as.numeric(data1$y)-1
ytest <- as.numeric(data2$y)-1

# separate predictors
x <- data.frame(data1[, c('x1', 'x2', 'x3')]) # if using `x1`, `x2`, and `x3` as predictors
xtest <- data.frame(data2[, c('x1', 'x2', 'x3')])

ensemble.model <- SuperLearner(y,
                               x,
                               family=binomial(),
                               SL.library=list("SL.randomForest", 
                                               "SL.ksvm") 
                               )
```

## Tune an ensemble model

An ensemble model needs to be tuned to improve performance. We can either tune the parameters of individual model or try different sets of algorithms.

For example, if we want to try some new parameters of above ensemble, we can do following,

```r
SL.randomForest.tune <- function(...){
      SL.randomForest(..., num.trees=200, mtry=2, nodeside=240)
    }

ensemble.model.tune <- SuperLearner(y,
                                    x,
                                    family=binomial(),
                                    SL.library=list("SL.ksvm",
                                                    "SL.randomForest.tune") 
                                    )
```

Of course, we can always try some other algorithms, which can be done easily using *SuperLearner*.

```r
ensemble.model.more <- SuperLearner(y,
                                    x,
                                    family=binomial(),
                                    SL.library=list("SL.ranger",
                                                    "SL.ksvm",
                                                    "SL.randomForest.tune",
                                                    "SL.glmnet") 
                                    )
```

In this case, we combine `ranger` and `glmnet` to previous ensemble, with the hope that it could improve the performance.

## Conclusion

Ensemble learning creates better performance by averaging, weighting different machine learning models and adding them together. It tends to work best when different individual models perform differently. Tuning an ensemble model is not easy and would require intuitions about different learners. This guide is just a simple instruction and a place to start. You should always try it on your own data to find more.

--------
[^1]: Vik Paruchuri. An Intro to Ensemble Learning in R. R-bloggers. https://www.r-bloggers.com/an-intro-to-ensemble-learning-in-r/
[^2]: Daniel Gremmell. Ensemble Learning in R with SuperLearner. DataCamp. https://www.datacamp.com/community/tutorials/ensemble-r-machine-learning
[^3]: https://github.com/ck37/superlearner-guide/blob/master/SuperLearner-Intro.Rmd

<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>
