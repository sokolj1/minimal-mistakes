---
header:
  image: /assets/2018-03-16-K-Means-Clustering/pokemon_background.jpg

author_profile: true

classes: wide

---

## Step by step how to build the renowned unsupervised machine learning algorithm from scratch

K-means Clustering is a type of unsupervised (no labeled data necessary) machine learning algorithm that determines optimal grouping, or clustering, amongst a dataset. This method is a great way to find patterns in your data. For instance, let's say you're working with the [Pokemon dataset](https://www.kaggle.com/abcsds/pokemon/data) by Alberto Barradas derived from the original Pokemon games (Gold, Silver, Ruby, Sapphire, etc):

{% highlight r %}

pokemon <- read.csv("pokemon.csv", sep = ",")
head(pokemon)

{% endhighlight %}

<img src="../assets/2018-03-16-K-Means-Clustering/k_means_1.png" align="center" > 

This dataset contains 12 features consisting of the number, name, first and second type, and basic statistics: Total Points, HP, Attack, Defense, Special Attack, Special Defense, Speed, Generation and Legendary status for exactly 800 pokemon (observations) describing the data. It turns out these pokemon statistics serve as a great introduction to unsupervised learning. 

We begin by plotting the relationship between Pokemon Speed and Defense using ggplot: 

{% highlight r %}

ggplot(pokemon, aes(x = Speed, y = Defense)) + geom_point() + 
ggtitle("Pokemon Species, Speed vs. Defense") +
xlab("Speed") + ylab("Defense")

{% endhighlight %}

<img src="../assets/2018-03-16-K-Means-Clustering/pokemon_init_ggplot.jpeg" align="center" > 

How would the K-means Clustering algorithm be applicable to this relationship of two pokemon features? If we are doing an analysis of pokemon to classify, or group, the pokemon that have optimal speed _and_ defense, K-means Clustering can determine the grouping of these pokemon. We will choose an arbitrary k value of 4. Once the K-means Clustering algorithm is rendered to the plot, we obtain the following result: 

{% highlight r %}

k <- 4
pokemon_speed_defense <- pokemon[,c(11,8)]
km.out <- kmeans(pokemon_speed_defense, centers = k, nstart = 20, iter.max = 50)

ggplot(pokemon_speed_defense, aes(x = Speed, y = Defense, color = factor(km.out$cluster))) 
+ geom_point() + labs(color = "Cluster") + ggtitle("Pokemon Species, Speed vs. Defense")

{% endhighlight %}

<img src="../assets/2018-03-16-K-Means-Clustering/pokemon_after_kmeans.jpeg" align="center" > 

It looks like the pokemon in cluster 2 have optimal defense and adequate speed, and the pokemon in cluster 3 have optimal speed and adequate defense. Although we have obtained our answer, I used the built in kmeans() R function to complete this task. But what happens behind the scenes with K-means clustering algorithm? How does it determine cluster assignments? All will be explained. 

## K-means Clustering Algorithm Steps 

The steps of the K-means Clustering algorithm is the following: 
1. Randomly create k points for starting cluster centers (centroids)
2. Each point in the dataset is assigned to the cluster of the nearest centroid based on closest euclidean distance
3. While any point has changed cluster assignment:
4. For every point in our dataset:
5. For every centroid:
6. Calculate the distance between the centroid and point
7. Assign the point to the cluster with the lowest distance
8. For every cluster, calculate the mean of the points in that cluster; assign this as the new coordinates of the centroid
9. Repeat until dataset cluster assignments do not change (error = 0)

The K-means Clustering function should have two parameters: dataset and desired number of clusters. The user determines the number of clusters to be assigned. Because of the random initial clustering assignments, some clustering outcomes may be better than others. But how can we tell which assignments are better than others?

You must be thinking "isn't there a quantitative way to measure the efficacy of K-means Clustering?" The answer is yes. The optimal clustering assignment will have the lowest total within sum of squares (twss) value. This is defined as the sum of the distance of every point to their assigned cluster center (centroid). Thinking about this visually, a dataset with a _low_ twss value will have their datapoints clumped together in obvious clusters, whereas not so obvious clusters will have a _high_ twss value. 

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



