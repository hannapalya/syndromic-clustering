# Clustering natural language chief complaints data for syndromic surveillance
*Work in Progress*

## Motivation

The COVID-19 pandemic demonstrated how critical early detection is for effective disease response. When novel pathogens emerge, traditional surveillance methods that rely on known symptom patterns can fail. By the time COVID-19 was identified as a novel disease, community transmission was already widespread.

Emergency departments (EDs) are often the first point of contact for patients with new diseases. Their chief complaints - the primary symptoms that brought them to the ED - contain valuable early warning signals. This project explores how modern NLP and clustering techniques can identify novel symptom patterns in ED data faster than traditional methods.

## Project Overview

This project implements unsupervised learning approaches to detect emerging symptom clusters in ED chief complaints. The core idea is that novel diseases will create unusual patterns in the symptom space that can be detected before the underlying cause is understood.

### Approach

The project explores vectorization and clustering strategies, of which two examples are included here:

1. BERT-DBSCAN:
   - Built for the MIMIC-IV-ED-DEMO dataset.
   - Uses Bio_ClinicalBERT embeddings to capture medical semantic relationships
   - Density-based clustering to find natural groupings
   - Automatically determines cluster numbers via density analysis
   - Well-suited for detecting outlier patterns that could indicate novel diseases
   
3. BioWordVec-KPrototypes:
   - Built for synthetic_data_2021.csv from Neill et al. 2023
   - Combines domain-specific word embeddings with patient metadata
   - Handles mixed numerical-categorical data (symptoms + demographics)
   - Fixed cluster number for more stable longitudinal analysis
   - Better for tracking known syndrome groups

## Technical Implementation

### Data Processing

1. Text Preprocessing
   - Clinical stopword removal
   - Punctuation normalization
   - Basic symptom phrase standardization

2. Embedding Generation
   - Bio_ClinicalBERT: 768-dimensional contextual embeddings
   - BioWordVec: 200-dimensional word vectors trained on PubMed/MIMIC
   - Sentence embeddings via mean pooling

3. Dimensionality Reduction
   - PCA with variance threshold (95%)
   - Typically reduces to 70-130 dimensions while preserving key patterns
   - Improves clustering speed and stability

4. Clustering
   - DBSCAN:
     - Epsilon selection via knee-point detection in k-distance graph
     - MinPts=4 (based on empirical testing)
     - Noise point detection for outlier analysis
   
   - K-Prototypes:
     - K=12 (determined via elbow method)
     - Categorical features: hospital code, age group
     - Gower distance for mixed-type similarity

### Evaluation Metrics

1. Cluster Quality:
   - Silhouette scores for cohesion/separation
   - Internal Gower distances
   - Noise point percentage (DBSCAN)

2. Time Series Analysis:
   - Pattern emergence tracking

3. Visualization:
   - 2D/3D PCA projections

## Current Results

- BERT-DBSCAN shows promise in detecting unusual symptom combinations
- Weekly analysis reveals temporal patterns in symptom clustering
- Further validation needed on known outbreak data
- Computational optimization required for real-time use


This is an active research project. Methods and implementations will continue to change. If you're interested in discussing this work, email me at hannapalya@gmail.com.

## References
[1] Johnson, A., Bulgarelli, L., Pollard, T., Horng, S., Celi, L. A., & Mark, R. (2023). MIMIC-IV (version 2.2). PhysioNet. https://doi.org/10.13026/6mm1-wy84

[2] Alsentzer, E., Murphy, J., Boag, W., Weng, W., Jin, D., Naumann, T., & McDermott, M. (2019). Publicly Available Clinical BERT Embeddings. In Proceedings of the 2nd Clinical Natural Language Processing Workshop (pp. 72-78). https://doi.org/10.18653/v1/W19-1909

[3] Neill, D. B. (2023). Pre-Syndromic Surveillance. GitHub. https://github.com/danielbneill/pre-syndromic-surveillance

