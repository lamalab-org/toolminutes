---
author: Sreekanth Kunchapu
date: 2024-05-29
bibliography: ./references_MolCLR.bib
---

# Molecular contrastive learning of representations (MolCLR) via graph neural networks

## Why discussing this paper? 

I chose Wang's et al.'s paper [@wang2022molecular] for our journal club because

- Labelled datasets are hard to acquire since conducting experiments is expensive and time-consuming.
- Labelled datasets are limited, if we train the model on the limited datasets, we might overfit the models and does not generalizable.
- The authors propose a framework to learn better representations with unlabelled datasets during pretraining, the pretained model is plugged in for the downstream tasks.

## Context

The paper introduces MolCLR, a self-supervised learning framework that uses graph encoders to enhance molecular representations effectively.

They considered a large unlabelled dataset with 10 million unique molecules SMILES from ChemBERTa and PubChem. This framework has two important steps. First, in the pre-training phase, they build molecule graphs and develop graph neural network encoders to learn differentiable representations. Second, the pretrained GNN backbone is used for the supervised learning tasks.

## Main idea behind contrastive learning
The aim of contrastive representation learning is to develop an embedding space where similar samples are positioned closely together, whereas dissimilar samples are kept distant from each other.

![Figure taken from the blog post by Ekin Tiu illustrating the idea behind contrastive learning.](MolCLR_2024_images/Constrative learning .png)

## NT-Xent loss

The authors use a NT-Xent loss to train the graph encoders.

$$
L_{ij} = -\log \left( \frac{\exp(\text{sim}(z_i, z_j)/\tau)}{\sum_{k=1}^{2N} \mathbf{1}_{\{k \neq i\}} \exp(\text{sim}(z_i, z_k)/\tau)} \right)
$$

- `z_i` and `z_j`: Latent vectors extracted from a positive data pair.
- `N`: Batch size, indicating the number of data pairs used.
- `sim(⋅)`: Function measuring the similarity between two vectors.
- `τ`: The temperature parameter, used to scale the similarity measures in the function.

To get more understanding, there is literature paper on contrastive learning one shoould read [@le2020contrastive].

## Overview of MolCLR framework

- The SMILES are converted into graphs in a batch. The graphs consists of nodes and edges. Nodes represents atoms and edges represents chemical bonds of a molecule.

- The authors studied three types of augmentations like atom masking, bond deletion and sub-graph removal.

- In the current work, they used sub-removal with some probability 0.25. Now, the model tries to learn hidden patterns of the molecule within the remaining subgraphs.

- The GNNs works on message passing framework, which means at each node the local neighborhood information is aggregated and updated iteratively. Finally, we obtain embeddings that poses every information about the molecule.

- This embeddings are fed forwarded to the MLP (Multi Layer preceptron) with hidden units to get latent representation of the molecule.

- The loss for the similar representations should be minimized and maximized for the disimilar representations.

![Overview figure for the MolCLR framework](MolCLR_2024_images/Overview figure MolCLR.png)

For more information about GNNs and its applications check out this review paper [zhou2020graph].

## Results
The authors tested the performance of MolCLR framework on seven benchmarks for classification task and six benchmarks for regression task.

The authors claim that on classification task with other self-supervised learning or pre-training strategies, their MolCLR framework achieves the best performance on five out of seven benchmarks, with an average improvement of 4.0 percentage.

![Table 1 shows the test performance on seven benchmarks on classifiaction task](MolCLR_2024_images/classifion benchmark.png) 

The authors also claim that on regression tasks, MolCLR surpasses other pre-training baselines in five out of six benchmarks and achieves almost the same performance on the remaining ESOL benchmark. For QM9 dataset, MolCLR does not have comparable performable with SchNet and MGCN. As these two models are specifically designed for quantum interaction and make use of extra 3D positional information.

![Table 2 shows the test performance on seven benchmarks on classifiaction task](MolCLR_2024_images/regression benchmark.png)

## Analysis of molecule graph augmentations
The authors employed four augmentation strategies on the classification benchamarks. In four of the augmentations, subgraph removal with probability 0.25 has better performance in (ROC-AUC %) all of the benchmarks except in BBBP benchmark.
The reason might be that model structures are sensitive in BBBP benchmark.

They also tested MolCLR framework on the same classification benchmark using GIN model with and without augmentation. With augmentations the model achieves superior performance than without augmentations.

![Figure 2 shows the investigation of molecule graph augmentations on classification benchmarks](MolCLR_2024_images/Augmentation analysis.png)

## Investigation of MOlCLR representation
The authors examined the representations learned by pretrained MOlCLR using t-SNE embedding. This method groups similar chemicals together in two-dimensional space. In Figure 3, they show a picture of 100,000 molecules from the validation set of PubChem database showing similar/dissimilar molecules learned by MolCLR pretraining. 

![Figure 3 Visualization of molecular representations learned by MolClr via t-SNe](MolCLR_2024_images/T-SNE visualization.png)

For instance, the three molecules shown on the top possess carbonyl groups connected with aryls. The two molecules shown on the bottom left have similar structures, where a halogen atom (fluorine or chlorine) is connected to benzene.

Therefore, leaerned representations are not random but they are meaningful representations without the label information.

In addition to this, the authors also query molecule from the PubcChem with ID 42953211 and the closest nine similar molecules are retrevied along with RDKFP and ECFP similarities labelled. It is observed that these selected molecules share the same functional groups, including alkyl halides (chlorine), tertiary amines, ketones and aromatics. A thiophene structure can also be found in all the molecules.

![Figure shows Comparison of MolCLR-learned representations and conventional FPs using the query molecule (PubChem ID 42953211)](MolCLR_2024_images/Query molecule.png)

## Take aways 

- Good molecular representations are important for better predictions
- Self-supervised learning would be an advantage for generalizable machine learning over supervised learning

## References

::: {#refs}
:::