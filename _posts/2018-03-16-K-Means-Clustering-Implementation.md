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

We will start by plotting the relationship between Pokemon Speed and Defense using ggplot: 

{% highlight r %}

P <- ggplot(pokemon, aes(x = Defense, y = Speed)) + geom_point()
P + ggtitle("Pokemon Species, Speed vs. Defense") +
xlab("Speed") + ylab("Defense")

{% endhighlight %}

<img src="../assets/2018-03-16-K-Means-Clustering/pokemon_init_ggplot.jpeg" align="center" > 

How would the K-means Clustering algorithm be applicable to this relationship of two pokemon features? If we are doing an analysis of pokemon to classify, or group, the pokemon that have optimal speed _and_ defense, K-means Clustering can be utilized to determine the grouping of these pokemon. Once the K-means Clustering algorithm is rendered to this plot, we obtain the following result: 

{% highlight r %}

pokemon_speed_defense <- pokemon[,c(11,8)]

km.out <- kmeans(pokemon_speed_defense, centers = k, nstart = 20, iter.max = 50)

ggplot(pokemon_speed_defense, aes(x = Defense, y = Speed, color = factor(Clusters))) + geom_point() + labs(color = "Cluster") + ggtitle("Pokemon Species, Speed vs. Defense")

{% endhighlight %}

<img src="../assets/2018-03-16-K-Means-Clustering/pokemon_after_kmeans.jpeg" align="center" > 

So it looks like the pokemon in cluster #1 are the pokemon that have optimized speed and defense! Although we obtained our answer, I used the built in kmeans() R function to complete this task. But what happens behind the scenes? What is the algorithm working in the background to determine the cluster assignments, and how does it work? All will be explained! 

## K-means Clustering Algorithm Steps 


