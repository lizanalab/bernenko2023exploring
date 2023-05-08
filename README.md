#     Mapping the semi-nested community structure of 3D chromosome contact networks

The goal of this project is to analyze the 3D structure of chromosomes using Hi-C data and network analysis techniques. Specifically, we aim to map out the chromosome's actual folding hierarchy by treating the measured DNA-DNA interactions as a weighted network and extracting 3D communities using the generalized Louvain algorithm.
# METHODS
## GenLouvain community detection
fill in

## Nestedness
fill in

## Hypergeometric test and enrichment folds
fill in

# DATA
## Hi-C data
In this project, we analyze Hi-C data from the human GM12878 B-lymphoblastoid cell line. We obtained published data from Gene Expression Omnibus (accession number is `GSE63525`). We normalized the Hi-C data using Knight-Ruiz Matrix Balancing (KR) algorithm implemented in `gcMapExplorer`.We present KR-normalized Hi-C data for chromosomes 3, 5, 10, and 22 in the folder `Hi-C_data_KR_norm_chr3-5-10-22`.

## 3D communities of the Hi-C network
In this project, the Hi-C data was treated as a network. Using the generalized Louvain algorithm, we tune its resolution parameter to scan through the community size spectrum, from A/B compartments to topologically associated domains (TADs), and then construct a hierarchical tree connecting these communities. The community partitions, as well as the partitions for the irreducible domains, are published in folder `3Dcommunities_GenLouvain_output_across_gamma`

## Reading data from files
Code snippet in Python to read Hi-C data from file `chr3_dna_00per.mat`

```python
import scipy.io as sio
chrname = "chr3"
file_path = "./specify/your/path/"
adj = sio.loadmat(file_path + chrname +"_dna_00per.mat")
adj = adj['data']
```

Code snippet in Python to read from file `chr3_gL_output.csv` that stores the community partitions for the range of gamma values (resolution parameter) and corresponding irreducible domains.

```python
import pandas as pd
chrname = "chr3"
file_path = "./specify/your/path/"
genlouvain_df = pd.read_csv(file_path+chrname+"_gL_output.csv", index_col=0)
```

Description of columns in files chrname+`_gL_output.csv`: 

| Column name      | Description |
| ----------- | ----------- |
| Index | ID of a Hi-C bin, given in a consecutive order from 0 to last |
| Bin_ID | the same as index |
| Bin_size(100kb) | the length of a DNA segment length. Each Hi-C bin has length of 1 (= 100 000 base pairs) |
| 0.9, …, 0.8, 0.7, 0.6, 0.5, 0.4, 0.3 | resolution parameter gamma for the GenLouvain algorithm. The column entries represent the community IDs for each bin_ID. If not given (NaN), then this bin was not assigned to a community |
| Dom_ID | Irreducible domains are numbered consecutively from 0 to the last domain. A domain is defined as a group of linearly adjacent Hi-C bins that are members of the same community for each tested gamma (0.9, …, 0.8, 0.7, 0.6, 0.5, 0.4, 0.3).|