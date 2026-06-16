# Mapping Meaning Across Indo-European Languages

This repository contains the code used for my master's thesis *Mapping Meaning Across Indo-European Languages: A Semantic Network Analysis Using Subs2Vec Embeddings**.
The project explores whether semantic networks created from word embeddings show structural similarities across Indo-European languages. In this analysis, words are represented as nodes, and semantic similarity relations between words are represented as edges. The resulting networks are then compared using graph-theoretic measures, hierarchical clustering, and visualizations.

This project uses pretrained **Subs2Vec** word embeddings by [van Paridon and Thompson](https://osf.io/preprints/psyarxiv/fcrmy_v1). Subs2Vec provides word embeddings and frequency data trained from OpenSubtitles corpora.

## Data

For each language, the workflow starts from two types of input files:

- a unigram frequency file;
- a word embedding file containing the word vectors.

In the preprocessing notebook, these files are expected in the following filename format:

```text
dedup.<language_code>.words.unigrams.tsv
subs.<language_code>.1e6.vec
```

For example for German: 

```text
dedup.de.words.unigrams.tsv
subs.de.1e6.vec
```

The raw Subs2Vec files are not included in this repository because of file size and copyright considerations. They can be downloaded from the original [Subs2Vec repository](https://github.com/jvparidon/subs2vec).

## Workflow

The notebooks are intended to be used in the following order:

1. `preprocess_unigrams.ipynb`  
   Cleans and aligns the Subs2Vec unigram frequency files with the embedding files. This step selects the top 10,000 most frequent words with valid 300-dimensional vectors for each language. It also normalizes token forms and handles duplicate token variants.

2. `cosinesim_knn_threshold.ipynb`  
   Computes pairwise cosine similarity matrices from the preprocessed embedding matrices. It then builds semantic networks using two methods:

   - **Threshold-based networks**: edges are retained when their cosine similarity is at or above the 95th percentile of the off-diagonal similarity values in each language-specific matrix.
   - **Mutual k-nearest-neighbor networks**: edges are retained only when two words appear in each other's top-k nearest-neighbor lists. In this project, the mutual kNN networks use `k = 1000 `.

3. `network_metrics.ipynb`  
   Calculates graph-theoretic metrics for each language network. These include:

   - number of nodes,
   - number of edges,
   - density,
   - average degree,
   - median degree,
   - degree standard deviation,
   - maximum degree,
   - number of connected components,
   - size of the largest connected component,
   - largest component ratio;
   - diameter of the largest connected component,
   - average shortest path length of the largest connected component,
   - average local clustering coefficient,
   - global transitivity,
   - modularity,
   - number of communities,
   - degree assortativity.

   Distance-based measures such as diameter and average shortest path length are calculated on the largest connected component, because disconnected nodes do not have finite paths between them. Community detection is performed with the Leiden algorithm.

4. `hierarchical_clustering.ipynb`  
   Uses selected network metrics to cluster languages and compare their semantic-network profiles. The clustering is performed separately for the threshold-based and mutual kNN networks.

5. `visualizations.ipynb`  
   Creates figures for comparing network measures across languages and network construction methods.

## Results

The final network metric table is available in `results/igraph_metrics_summary.csv´. This file can be used to inspect the calculated graph-theoretic measures and to reproduce the hierarchical clustering and visualization steps without rerunning the full preprocessing and network construction pipeline.
Large intermediate files are not included in this repository. This includes raw embedding files, preprocessed `.npz` embedding matrices, cosine similarity matrices, and GraphML network files.

## Notes

This repository documents the analysis workflow used for the thesis. The thesis will be linked here once it is publicly available.