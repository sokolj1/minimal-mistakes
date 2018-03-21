---
header:
  image: /assets/2018-03-16-K-Means-Clustering/pokemon_background.jpg

author_profile: true

classes: wide

---

## Step by step how to build the renowned unsupervised machine learning algorithm from scratch

K-means Clustering is a type of unsupervised (no labeled data necessary) machine learning algorithm that determines optimal grouping, or clustering, amongst a dataset.  For instance, lets say your working with the [Pokemon dataset](https://www.kaggle.com/abcsds/pokemon/data) by Alberto Barradas derived from the original Pokemon games (Gold, Silver, Ruby, Sapphire, etc):

{% highlight r %}

pokemon <- read.csv("pokemon.csv", sep = ",")
head(pokemon)

{% endhighlight %}

<img src="../assets/2018-03-16-K-Means-Clustering/k_means_1.png" align="center" > 

This dataset contains 12 features consisting of the number, name, first and second type, and basic statistics: Total Points, HP, Attack, Defense, Special Attack, Special Defense, Speed, Generation and Legendary status for exactly 800 pokemon (observations) describing the data. It turns out that these pokemon statistics serve as a great introduction to unsupervised learning. 

We start by plotting the relationship between Pokemon Speed and Defense using ggplot: 

{% highlight r %}

ggplot(pokemon, aes(x = Defense, y = Speed)) + geom_point() + 
ggtitle("Pokemon Species, Speed vs. Defense") +
xlab("Speed") + ylab("Defense")

{% endhighlight %}

<img src="../assets/2018-03-16-K-Means-Clustering/pokemon_init_ggplot.jpeg" align="center" > 

How would the K-means Clustering algorithm be applicable to this relationship of two pokemon features? If we are doing an analysis of pokemon to classify, or group, the pokemon that have optimal speed _and_ defense, K-means Clustering can be utilized to determine the grouping of these pokemon. Once the K-means Clustering algorithm is rendered to this plot, we obtain the following result: 

{% highlight r %}

pokemon_speed_defense <- pokemon[,c(11,8)]
km.out <- kmeans(pokemon_speed_defense, centers = k, nstart = 20, iter.max = 50)

ggplot(pokemon_speed_defense, aes(x = Defense, y = Speed, color = factor(Clusters))) 
+ geom_point() + labs(color = "Cluster") + ggtitle("Pokemon Species, Speed vs. Defense")

{% endhighlight %}

<img src="../assets/2018-03-16-K-Means-Clustering/pokemon_after_kmeans.jpeg" align="center" > 

So it looks like the pokemon in cluster #1 are the pokemon that have optimized speed and defense! Although we have obtained our answer, I used the built in kmeans() R function to complete this task. But what happens behind the scenes? What is the algorithm working in the background to determine the cluster assignments, and how does it work? All will be explained. 

## K-means Clustering Algorithm Steps 

The steps of the K-means Clustering algorithm is the following: 
1. Randomly assign each point to one of the k clusters 
2. Calculate the centers (or centroids) of each of the clusters
3. Each point in the dataset is assigned to the cluster of the nearest centroid based on euclidean distance
4. Recalculate coordinates of centroid again with new dataset cluster assignments 
5. Repeat until dataset cluster assignments do not change (error = 0)

To begin, the K-means Clustering function should have two parameters: dataset and desired number of clusters. The user determines the number of clusters to be assigned. Because of the random initial clustering assignments, some clustering outcomes may be better than others. But how can you tell which assignments are better than others?

You must be thinking "there must be a quantitative way to measure the efficacy of K-means Clustering?" The answer is yes. The optimal clustering assignment will have the lowest total within sum of squares (twss) value. This is defined as the sum of the distance of every point to their assigned cluster center (centroid). So thinking about this visually, a dataset with a _low_ twss value will have their datapoints clumped together in obvious clusters, whereas not so obvious clusters will have a _high_ twss value. 

Lets define a function that calculates euclidean distance, which is the distance formula from high school algebra:

{% highlight r %}

euclid_dist <- function(x, y) {
  
    # initialize distance to 0    
    distance = 0  
    
    # loops by number of columns in x (should be 2)
    for (i in (1:length(x))) {  
    
        # subtract each value from their respective columns; square the difference, then add this value to distance
        distance = distance + (x[[i]] - y[[i]])^2                                                    
    }
    
    # square root of the distance
    distance = sqrt(distance)  
    return (distance)
}

{% endhighlight %}

Now that we have our euclidean distance function defined, we can dive into the actual K-means Clustering function that does the heavylifting. 

{% highlight r %}

# function that calculates k nearest clusters of a dataset
# km_data = input dataset defined as km_data[1] = x, km_data[2] = y, k = number of clusters
km_function <- function(km_data, k) {
  
    # initializing random cluster datapoints based on points from dataset
    # concatentate the x and y coordinates into data frame 
    cluster_loc <- km_data[sample.int(nrow(km_data),k),]
    
    # initialize data frame to store old cluster coordinates
    cluster_loc_old <- as.data.frame(matrix(0, ncol = 2, nrow = k))
    
    # empty data frames to store datapoint cluster ID (0, 1, 2, ...) and datapoint of particular cluster assignment
    cluster_id <- vector("numeric", nrow(km_data))
    clu_assign = as.data.frame(matrix(0, ncol = 2, nrow = nrow(km_data)))
    wss <- vector("numeric")
    
    # error: the distance between previous cluster and new calculated cluster locations 
    # algorithm reaches convergence when clusters no longer change (error = 0)
    error <- sum(euclid_dist(cluster_loc, cluster_loc_old))

{% endhighlight %}

The first line of code inside the function randomly determines the initial starting point for the k clusters. The sample.int() function takes k random data values and assigns them as the initial clusters in cluster_loc. 



