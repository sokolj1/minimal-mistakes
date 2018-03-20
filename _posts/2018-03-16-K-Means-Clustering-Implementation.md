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

{% endhighlight %}

