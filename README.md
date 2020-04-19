<meta name='keywords' content='Text clustering, Affinity Propagations, TF-IDF'>
  
![Text Clustering](https://media3.picsearch.com/is?DcTQdjuy5bQhzMPUL1d85olnj0YkiW0oZz_Y739XI2U&height=284)

# COVID-19 Literature Clustering
COVID-19 literature clustering using Affinity Propagation (the number of clusters is unknown in advance)

## Motivation
[COVID-19](https://en.wikipedia.org/wiki/Coronavirus_disease_2019) as abbreviated Coronovirus Disease 2019 led to the surge in research publications intended to help battling this deadly infection. [Kaggle](https://www.kaggle.com) arranged the [COVID-19 Open Research Dataset Challenge (CORD-19)](https://www.kaggle.com/allen-institute-for-ai/CORD-19-research-challenge/) offering dozens of thousands articles to analyze in an automatic way.

The most straightforward approach is first to cluster articles into groups so that the research focus can be narrowed when a specific research question is asked. [K-means](https://en.wikipedia.org/wiki/K-means_clustering) is a natural choice for doing this. However, it requires the number of clusters to be pre-specified in advance. In practice, this number could only be guessed. Of course, one could still run the K-means algorithm with different values of k and then select the k value optimal according to a certain criterion. Given the large number of articles, I dismiss such an approach in favor of clustering that does not need to set the number of clusters in advance. In other words, the optimal number of clusters is decided in the process of text clustering by an algorithm itself.

## Data

## Approach
### Challenge
All articles are on the same subject!

## Results

## Discussion
