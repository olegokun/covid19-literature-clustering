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
The challenge to detect clusters is that all articles are written on the same **main** topic, though there are a number of sub-topics that a human can easily detect, while this is not so easy by a computer. Hence, a clustering algorithm needs to be somewhat sensitive to minor details allowing smaller clusters to be discovered, rather than to group all articles into one big cluster. With this goal in mind and aiming at clustering with unknown number of clusters, I turned my attention to [Affinity Propagation](https://en.wikipedia.org/wiki/Affinity_propagation), based on the comparison of various clustering algorithms, found [here](https://scikit-learn.org/stable/auto_examples/cluster/plot_cluster_comparison.html#sphx-glr-auto-examples-cluster-plot-cluster-comparison-py).

Nevertheless, even Affinity Propagation might fail to discern fine details in such a large dataset. Therefore I resorted to a two-stage approach. 

In the first stage, I randomly selected a small subset of articles and group them with Affinity Propagation. The motivation was that a clustering algorithm would discover multiple clusters, no matter how small they are, when a dataset is small and articles are randomly picked. The goal was to determine the number of clusters, find their centroids and extract the top N words characterizing each cluster.

In the second stage, using the found clusters as "seeds", the rest of articles are added (one-by-one) to the clusters based on the closest centroid principle. The list of top N words describing each cluster is updated each time when a new article is assigned to a cluster, so that when all 30K articles are processed, such lists can be used to narrow down search for answers to a particular practical question.

## Comments to code
All code is within a single [Jupyter notebook](https://github.com/olegokun/covid19-literature-clustering/blob/master/Data%20Clustering%20with%20Unknown%20Number%20of%20Clusters%20v1.ipynb).

Loading text from JSON files is adopted from earlier contributors as acknowledged in the notebook comments. Main fields of interest are 'paper_id', 'abstract' and 'body_text'. For convenience, I merged the latter two into a single field 'text'.

As later versions of the dataset included articles witten in other languages rather than English, the *fasttext* package is used to identify language and to retain only articles whose language is English. To use *fasttext*, one needs to download the pre-trained model *lid.176.ftz* by following the link in my notebook. 

The battery of text pre-processing techniques in function `normalized_corpus` cleans up text, thus preserving only nouns and adjectives, which top 100 [bi-grams](https://en.wikipedia.org/wiki/Bigram) are extracted from. The list of bi-grams for each article is flattened out before further processing. Due to multiple operations, text pre-processing is the most time-consuming part of the entire pipeline.

As raw text needs to be transformed into a digital form, [TF-IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf) features are extracted from the flattened bi-grams. When doing text clustering on TF-IDF, top 30 words are assigned to each cluster that best characterize it. Cluster analysis also includes [word cloud](https://en.wikipedia.org/wiki/Tag_cloud#Text_cloud) generation for each cluster.

## Results

Affinity Propagation clustering of 100 articles randomly selected for cluster seeding reveals 16 small clusters where any cluster does not exceed in size other clusters. Initially, words describing clusters may look a bit noisy as it seems sometimes certain surnames were included as shown below:
```
Cluster 1 details:
----------------------------------------------------------------------------------------------------
Key features:  ['al', 'fel', 'et', 'infantum', 'maia', 'brianti', 'basso', 'pennisi', 'gallego', 'clinical', 'disease', 'robert', 'manzillo', 'pimenta', 'richter', 'hervás', 'dedola', 'magalon', 'fileccia', 'bardagi', 'leishmania', 'attipa', 'feline', 'chatzis', 'solano', 'mattos', 'laruelle', 'rüfenacht', 'blood', 'high']
#articles in a cluster:  13
```

However, once many more articles are added to this cluster, top words better concentrate on "technical" content:
```
Cluster 1 details:
----------------------------------------------------------------------------------------------------
Key features:  ['al', 'et', 'protein', 'virus', 'cell', 'viral', 'infection', 'rna', 'activity', 'acid', 'antibody', 'human', 'expression', 'immune', 'disease', 'replication', 'gene', 'analysis', 'type', 'ifn', 'domain', 'sequence', 'membrane', 'animal', 'response', 'amino', 'respiratory', 'genome', 'mice', 'coronavirus']
#articles in a cluster:  2098
```

Despite the initial goals, the clustering produces one large cluster comprised of 24,000 articles:
```
Cluster 8 details:
----------------------------------------------------------------------------------------------------
Key features:  ['virus', 'health', 'infection', 'disease', 'cell', 'respiratory', 'viral', 'activity', 'influenza', 'protein', 'human', 'analysis', 'age', 'al', 'control', 'case', 'group', 'clinical', 'care', 'expression', 'antibody', 'acute', 'blood', 'acid', 'risk', 'high', 'study', 'immune', 'response', 'rna']
#articles in a cluster:  24009
```

For comparison, this is how the top 30 words looked like for this cluster after the "seed" set clustering.
```
Cluster 8 details:
----------------------------------------------------------------------------------------------------
Key features:  ['emergence', 'city', 'spatial', 'probability', 'number', 'heterogeneity', 'human', 'population', 'intermediate', 'event', 'infectious', 'em', 'size', 'pathogen', 'village', 'extinction', 'homogeneity', 'infinite', 'host', 'evolutionary', 'strain', 'novel', 'mutation', 'route', 'effect', 'analytical', 'reproductive', 'zoonotic', 'general', 'outbreak']
#articles in a cluster:  4
```

In addition to cluster analysis, model objects for the TF-IDF vectorizer and the Affinity Propagation clusterer as well as results of text pre-processing and clustering are saved in files. The word clouds for clusters are also saved in PNG files named *cluster_i*, where *i* ranges from 0 to 15.

## Discussion
Even though the majority of articles were still assigned to a single cluster, thus emphasizing the difficulty of automatic differentiation between articles written on very similar topics, smaller but still significant clusters like Cluster 1 could be useful in answering research questions.

The two-stage clustering is believed to lead to better results than trying to cluster all articles at once, because it preserves small clusters ("seeds"), where larger and more meaningful clusters can emerge from when more articles are added.
