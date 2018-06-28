---
layout: post
title: Clutering in R
---

## What is Clustering

In machine learning, there are types of problems, namely, regression and classification. Clustering is a major type of classification that separate data points into different aggreates or clusters. 

A major feature of clustering is that it is unsupervised, which means no pre-defined label is needed. Instead, the algorithm will learn the similarity of data points based on some heuretics.

## K-means Clustering

The most well known clustering method is K-means clustering. It partitions $$n$$ observations into $$k$$ clusters, where each observation belongs to the cluster with nearest mean. The partition process is done via minimizing the within-cluster sum of square (WCSS), i.e.,

$$\text{argmin}_S\sum_{i=1}^{k}\sum_{x\in S_i} ||x-\mu_i||^2 = \text{argmin}_S\sum_{i=1}^{k}|S_i|Var(S_i)$$

where \\( \mu_i \\) is the mean of points in \\( S_i \\). Here, distance is measured as Euclidean distance. Other distance measures are also applicable.

Assume we have a training dataset name `data1`, which contains many rows and several columns (let's assume these columns named `y`, `x1`, `x2`, etc. where `y` is a factor variable for classification; you can try some real datasets such as the famous `iris` dataset). Assume the test dataset name `data2`. We can build a clustering model as following,

```r
require(ggplot)

# clustering
set.seed(20)
clustering <- kmeans(data1, 3, nstart=20)
freq_table <- table(clustering$cluster)

print(clustering$centers)
print(clustering$size)

clustering$cluster <- as.factor(clustering$cluster)
ggplot(temp, aes(num.death, viol.quantity, color = clustering$cluster)) + geom_point()
```

## Spectral Clustering

Unlike K-means, spectral clustering is to cluster data based on the connectivity, rather than the mean. Spectral clustering uses the similarity matrix between point $$x$$ to point $$y$$ to cluster the data points. The usual similarity measure is the Gaussian kernel with Euclidean distance. Once similarity matrix is determined, we can apply KNN to get a graph representation of the data points.

Now the clustering problem becomes a graph partition problem. Spectral clustering tries to construct a cluster where edges connecting nodes in different clusters with low weights and edges connecting nodes within the same cluster with high weights[^1].

```r
require(kernlab) # package for the spectral clustering

set.seed(20)
sc.model <- specc(data1, centers=3)

plot(data1, col=sc.model, pch=4) # plot clusters
```

## Conclusion

Clustering is a major unsupervised classification algorithm. Here, we present two specific ones, namely, K-means and spectral clustering. K-means clusters data points by minimizing WCSS. On the other hand, spectral clustering reduces the clustering problem to a graph partition problem. This guide serves a simple introduction to clustering algorithms. 

Find more in [^3] and [^4].

--------
[^1]: Joao Neto. Spectral Clustering. http://www.di.fc.ul.pt/~jpn/r/spectralclustering/spectralclustering.html
[^2]: K-means Clustering. Wikipedia. https://en.wikipedia.org/wiki/K-means_clustering
[^3]: Spectral clustering详解. https://www.cnblogs.com/Leo_wl/p/3156049.html
[^4]: Spectral clustering方法总结. https://blog.csdn.net/u010967291/article/details/53465645


<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>
