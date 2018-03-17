---
header:
  image: /assets/perceptron_neuron.png

author_profile: true

classes: wide

---

## Understanding the linearly separable binary classifier from the ground up using R

The perceptron. It has become a rite of passage for comprehending the underlying mechanism of neural networks, and machine learning as a whole. The algorithm was invented in 1957 at the Cornell Aeronautical Laboratory by Frank Rosenblatt with funding from the US government [[1]](http://psycnet.apa.org/doiLanding?doi=10.1037%2Fh0042519). The concept behind the perceptron is simple: separate two distinct categories of data based on linear separability. This tutorial uses data that has 2 dimensions, which can be downloaded [here](assets/2018-03-04-Single-Layer-Perceptron-Implementation/percep_data.csv). Lets start by importing the data into RStudio, then dividing the dataset into separate train and test DataFrames (80% train, 20% test). 


{% highlight r %}
# imports csv file
percep_data <- read.csv("percep_data.csv", sep = ",")

# divides complete dataset into test and train datasets
indexes <- sample(1:nrow(percep_data), size = 0.8 * nrow(percep_data))
train <- percep_data[indexes,]
test <- percep_data[-indexes,]

{% endhighlight %}

Our data is now ready to be classified. But as always, exploratory data analysis (EDA) is an important process that should not be overlooked. We can plot the train set with the known labels so we know what to expect of our implemented perceptron function.

{% highlight r %}

str(train)
with(train, plot(X1, X2, col = Y + 3, xlab = "Feature 1", ylab = "Feature 2", main = "Train Dataset with Known Labels"))

{% endhighlight %}

<img src="../assets/2018-03-04-Single-Layer-Perceptron-Implementation/init_percep_plot.jpg" align="center" > 

It looks like with the exception of a few outliers, the data is linearly separably. Great! We can now proceed to learn the intracacies of this neural network classifier. 

# The Neural Network "Black Box" Explained

This single neural network node accepts two input values, which in this case, is X1 and X2 of the train data. These two features are multiplied by corresponding "weights" that play a key role in classification. The products are subsequently added together, as shown below: 

{% highlight r %}

X1 * weight1 + X2 * weight2

{% endhighlight %}

What happens next? The sum is put through an _activation_ function, which is arbitarily chosen by the data scientist based on the neural network. A popular activation function is the ReLu activation function, but for the perceptron, we will use the step function. If the sum is negative ( < 0), the value put through the step function is -1; for 0, the value is 0; for positive values ( > 0) the value is 1. 

If you noticed during the EDA process, the known binary labels (Y) are -1 and 1, so the step function is convenient for predicting the labels of our data. If you take a hard look at the perceptron node model, the weights play a significant role regarding determining label accuracy. Certainly it is unlikely that the weights are initialized to the ideal weight values that yield correct classification. For this function, the weights are initialized to 0, and the function "learns" from the data to gradually correct the weight values until linear separability is obtained. 

# Perceptron Implementation 

It is time to dive into the code of the perceptron implementation. Define a global vector to store the prediction labels. Inside the perceptron function, initialize the two weight values to 0. The bias is also initialized to 0. The bias accounts for the distance of the separator line from the origin. 

{% highlight r %}

prediction <- vector("integer", nrow(train))

percep_function <- function (train) {
    weightX1  <- 0
    weightX2  <- 0
    bias      <- 0

{% endhighlight %}

The function uses a while loop to iteratively repeat the fine tuning of the weights and bias values
until label classification reaches the desired threshold, which I set for my function to 94% accuracy. 
This step is important to understand. Notice with the first plot "Train Dataset with Known Labels" there are outliers 
on both sides of the linear separator. If the perceptron function was set to predict with 100% accuracy, 
the function would never reach convergence, and the while loop would repeat infinitely. To circumvent this issue, the 
function is set for 94% accuracy (an A for a majority of college level classes). 

