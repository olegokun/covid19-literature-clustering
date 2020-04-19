<meta name='keywords' content='Text clustering, Affinity Propagations, TF-IDF'>
  
![Document Clustering](https://media3.picsearch.com/is?DcTQdjuy5bQhzMPUL1d85olnj0YkiW0oZz_Y739XI2U&height=284)

# COVID-19 Literature Clustering
COVID-19 literature clustering using Affinity Propagation (the number of clusters is unknown in advance)

## Motivation
[COVID-19](https://en.wikipedia.org/wiki/Coronavirus_disease_2019) as abbreviated Coronovirus Disease 2019 led to the surge in research publications intended to help battling this deadly infection. [Kaggle](https://www.kaggle.com) arranged the [COVID-19 Open Research Dataset Challenge (CORD-19)](https://www.kaggle.com/allen-institute-for-ai/CORD-19-research-challenge/) offering dozens of thousands articles to analyze in an automatic way.

The most straightforward approach is first to cluster articles into groups so that the research focus can be narrowed when a specific research question is asked. [K-means](https://en.wikipedia.org/wiki/K-means_clustering) is a natural choice for doing this. However, it requires the number of clusters to be pre-specified in advance. In practice, this number could only be guessed. Of course, one could still run the K-means algorithm with different values of k and then select the k value, optimal according to a certain criterion. Given the large number of articles, I dismiss such an approach in favor of clustering that does not need to set the number of clusters in advance. In other words, the optimal number of clusters is decided in the process of text clustering by an algorithm itself.

## Data
The article dataset has been augmented several times during he above mentioned Challenge. I downloaded the dataset comprising about 30,000 articles. Though there are only articles written in English among them, I took a provision to identify the language and to filter out non-English articles that were later (perhaps, accidently) added by Challenge organizers. I didn't uploaded the articles as this could easily be done by clicking on the link of the Challenge.

## Approach
The challenge to detect clusters is that all articles are written on the same **main** topic, though there are a number of sub-topics that a human can easily detect, while this is not so easy by a computer. Hence, a clustering algorithm needs to be somewhat sensitive to minor details allowing smaller clusters to be discovered, rather than to group all articles into one big cluster. With this goal in mind and aiming at clustering with unknown number of clusters, I turned my attention to [Affinity Propagation](https://en.wikipedia.org/wiki/Affinity_propagation) based on the comparison of various clustering algorithms, found [here](https://scikit-learn.org/stable/auto_examples/cluster/plot_cluster_comparison.html#sphx-glr-auto-examples-cluster-plot-cluster-comparison-py).

Nevertheless, even Affinity Propagation might fail to discern fine details in such a large dataset. Therefore I resorted to a two-stage approach. 

In the first stage, I randomly selected a small subset of articles and group them with Affinity Propagation. The motivation was that a clustering algorithm would discover multiple clusters, no matter how small they are, when a dataset is small and articles are randomly picked. The goal was to determine the number of clusters, find their centroids and extract the top N words characterizing each cluster.

In the second stage, using the found clusters as "seeds", the rest of articles are added (one-by-one) to the clusters based on the closest centroid principle. The list of top N words describing each cluster is updated each time when a new article is assigned to a cluster, so that when all 30K articles are processed, such lists can be used to narrow down search for answers to a particular practical question.

## Comments to code

## Results

## Discussion
