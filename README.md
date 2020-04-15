# Unsupervised-Learning-with-R
to help explain the repo i got a tutorial from datacamp that explains clustering in an awesome way.


As the name itself suggests, Clustering algorithms group a set of data points into subsets or clusters. The algorithms' goal is to create clusters that are coherent internally, but clearly different from each other externally. In other words, entities within a cluster should be as similar as possible and entities in one cluster should be as dissimilar as possible from entities in another.



Broadly speaking there are two ways of clustering data points based on the algorithmic structure and operation, namely agglomerative and divisive.


Agglomerative : An agglomerative approach begins with each observation in a distinct (singleton) cluster, and successively merges clusters together until a stopping criterion is satisfied.
Divisive : A divisive method begins with all patterns in a single cluster and performs splitting until a stopping criterion is met.
In this tutorial you are going to focus on the agglomerative or bottom-up approach, where you start with each data point as its own cluster and then combine clusters based on some similarity measure. The idea can be easily adapted for divisive methods as well.

The similarity between the clusters is often calculated from the dissimilarity measures like the euclidean distance between two clusters. So the larger the distance between two clusters, the better it is.

There are many distance metrics that you can consider to calculate the dissimilarity measure, and the choice depends on the type of data in the dataset. For example if you have continuous numerical values in your dataset you can use euclidean distance, if the data is binary you may consider the Jaccard distance (helpful when you are dealing with categorical data for clustering after you have applied one-hot encoding). Other distance measures include Manhattan, Minkowski, Canberra etc.


# Pre-processing operations for Clustering
There are a couple of things you should take care of before starting.

Scaling
It is imperative that you normalize your scale of feature values in order to begin with the clustering process. This is because each observations' feature values are represented as coordinates in n-dimensional space (n is the number of features) and then the distances between these coordinates are calculated. If these coordinates are not normalized, then it may lead to false results.

For example, suppose you have data about height and weight of three people: A (6ft, 75kg), B (6ft,77kg), C (8ft,75kg). If you represent these features in a two-dimensional coordinate system, height and weight, and calculate the Euclidean distance between them, the distance between the following pairs would be:

A-B : 2 units

A-C : 2 units

Well, the distance metric tells that both the pairs A-B and A-C are similar but in reality they are clearly not! The pair A-B is more similar than pair A-C. Hence it is important to scale these values first and then calculate the distance.

There are various ways to normalize the feature values, you can either consider standardizing the entire scale of all the feature values (x(i)) between [0,1] (known as min-max normalization) by applying the following transformation:

x(s)=x(i)−min(x)/(max(x)−min(x))
You can use R's normalize() function for this or you could write your own function like:

standardize <- function(x){(x-min(x))/(max(x)-min(x))}

Other type of scaling can be achieved via the following transformation:

x(s)=x(i)−mean(x)/sd(x)
Where sd(x) is the standard deviation of the feature values. This will ensure your distribution of feature values has mean 0 and a standard deviation of 1. You can achieve this via the scale() function in R.

Missing Value imputation
It's also important to deal with missing/null/inf values in your dataset beforehand. There are many ways to deal with such values, one is to either remove them or impute them with mean, median, mode or use some advanced regression techniques. R has many packages and functions to deal with missing value imputations like impute(), Amelia, Mice, Hmisc etc. You can read about Amelia in this tutorial.

Hierarchical Clustering Algorithm


The key operation in hierarchical agglomerative clustering is to repeatedly combine the two nearest clusters into a larger cluster. There are three key questions that need to be answered first:

How do you represent a cluster of more than one point?
How do you determine the "nearness" of clusters?
When do you stop combining clusters?
Hopefully by the end this tutorial you will be able to answer all of these questions. Before applying hierarchical clustering let's have a look at its working:

It starts by calculating the distance between every pair of observation points and store it in a distance matrix.
It then puts every point in its own cluster.
Then it starts merging the closest pairs of points based on the distances from the distance matrix and as a result the amount of clusters goes down by 1.
Then it recomputes the distance between the new cluster and the old ones and stores them in a new distance matrix.
Lastly it repeats steps 2 and 3 until all the clusters are merged into one single cluster.
There are several ways to measure the distance between clusters in order to decide the rules for clustering, and they are often called Linkage Methods. Some of the common linkage methods are:

Complete-linkage: calculates the maximum distance between clusters before merging.
