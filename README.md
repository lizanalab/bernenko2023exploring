#     Exploring 3D community inconsistency in human chromosome contact networks

_Dolores Bernenko, Sang Hoon Lee, Ludvig Lizana_

We utilize Hi-C data and network science tools to examine the 3D structure of DNA. In our approach, Hi-C data is interpreted as a weighted network where nodes represent DNA segments and edges denote contacts between these segments. To identify mesoscale communities within these networks, we employ the community detection technique known as GenLouvain.

A pivotal part of our research involves investigating the inherent inconsistency of community structures in Hi-C data across all human chromosomes. To delve into this, we base our analysis on two inconsistency metrics, both local and global. We then identify the network scales most susceptible to variations in community structure.

We encourage users to contact the developers should problems arise.

## Hi-C data

In this project, we analyze Hi-C data from the human GM12878 B-lymphoblastoid cell line. We obtained published data from Gene Expression Omnibus (accession number is `GSE63525`). We normalized the Hi-C data using Knight-Ruiz Matrix Balancing (KR) algorithm implemented in `gcMapExplorer`.  We present KR-normalized Hi-C data for chromosomes 3, 5, 10, and 22 in the folder `data/Hi-C_data_KR_norm_chr3-5-10-22`.

## Community detection using GenLouvain

The Hi-C data was treated as a network. Using the generalized Louvain algorithm, we tune its resolution parameter to scan through the community size spectrum, from A/B compartments to topologically associated domains (TADs). In this repository, we provide code for community detection approach.

With the provided MATLAB implementation of GenLouvain (`methods/GenLouvain`), users can apply the algorithm to their own KR-normalized Hi-C datasets to identify communities of various size scales.

_Script location:_ ```methods/GenLouvain/```<br>
_Script name:_ ```genLouvain_community_detection.m```<br>

_Description:_ This MATLAB script detects communities in chromosomes' Hi-C data using the GenLouvain community partition method.

_I. How to use this script?_<br>
To use this code, first specify which Hi-C data (chromosome name) will be partitioned into communities.
```matlab
chr1 = 10; %choose the first chromosome
chr2 = 10; %choose the second chromosome (for interchromsomal Hi-C) or repeat the first one (for intrachromosomal Hi-C)
```

Next, ensure that the Hi-C data and chromosome length information are available in the specified paths:
```matlab
data_path = ['/MATLAB Drive/mapping2023bernenko/HiC_data/chr' num2str(chr1) '_dna_00per.mat']
info_path = ['/MATLAB Drive/mapping2023bernenko/HiC_data/chr_' num2str(chr1) '.data.info']
```
For example, data for chromosome 10 are in files ```chr10_dna_00per.mat``` and ```chr_10.data.info```, and both files are located in folder ```methods/GenLouvain/HiC_data```

Afterwards, set the parameters:<br>
```alpha```: The exponent of the decay of contact frequencies between node i and node j. Default value is 1<br>
```gamma```: The resolution parameter, which can be set to a single value (for example 0.6) or a range of values (example is below).
```matlab
alpha = 1; % FIX one parameter

for gamma = 0.6% [0.4, 0.5, 0.6, 0.7, 0.8, 0.81, 0.82, 0.83, 0.84, 0.85, 0.86, 0.87, 0.88, 0.89, 0.9] ```
```
Last, run the algorithm. It will save the data into folder ```methods/GenLouvain/gL_partitions```

_II. Output_<br>
The script saves the community assignments for nodes in a .mat file in the gL_partitions directory, with the naming format chr{chr1}_gamma{gamma_n}_alpha{alpha_n}.mat.<br>

The output file contains:<br>
**ROWS**<br>
representing nodes;<br>
**COLUMNS**<br>
_column#1_ contains nodes' community assignment,<br>
_column#2_ is a chromosome name,<br>
_column#3_ is a modularity of the partition (not normalized).