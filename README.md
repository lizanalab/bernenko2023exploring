#     Mapping the semi-nested community structure of 3D chromosome contact networks

The goal of this project is to analyze the 3D structure of chromosomes using Hi-C experiments and network analysis techniques. Specifically, we aim to map out the chromosome's actual folding hierarchy by treating the measured DNA-DNA interactions as a weighted network and extracting 3D communities using the generalized Louvain algorithm.

The dataset used in this project is GenLouvain community partitions of the Hi-C data:

**_NOTE:_** In this project, we analyze Hi-C data from the human GM12878 B-lymphoblastoid cell line. We obtained the published data  from Gene Expression Omnibus (accession number is GSE63525). We normalized the Hi-C data using Knight-Ruiz Matrix Balancing (KR) algorithm implemented in gcMapExplorer. We publish KR-normalized Hi-C data for chromosomes 3, 5, 10, 22 in folder `Hi-C_data_KR_norm_chr3-5-10-22`

This Hi-C data is then treated as a network. By utilizing the resolution parameter of the generalized Louvain algorithm, we can scan seamlessly through the community size spectrum, from A/B compartments to topologically associated domains (TADs), and construct a hierarchical tree connecting these communities.

**_NOTE:_** The community partitions, as well the partitions for the irreducible domains, are stored in folder `3Dcommunities_GenLouvain_output_across_gamma`

## Reading data

Code snippet in Python to read Hi-C data from file `chr3_dna_00per.mat`

```python
import scipy.io as sio
chrname = "chr3"
file_path = "./specify/your/path/"
adj = sio.loadmat(file_path + chrname +"_dna_00per.mat")
adj = adj['data']
```

Code snippet in Python to read from file `chr3_gL_output.csv` storing community partitions for the range of gamma values and corresponding irreducible domains.

```python
import pandas as pd
chrname = "chr3"
file_path = "./specify/your/path/"
genlouvain_df = pd.read_csv(file_path+chrname+"_gL_output.csv", index_col=0)
```