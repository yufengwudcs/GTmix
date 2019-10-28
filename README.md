# GTmix
Inference of population admixture network from local gene genealogies under coalescent theory

GTmix is a computer program for inferring population admixture networks from inferred local gene genealogies. If you use GTmix, please cite:

Inference of Population Admixture Network from Local Gene Genealogies: a Coalescent-based Maximum Likelihood Approach, Yufeng Wu, manuscript, 2019.

Population admixture network is a model for population evolution. Admixture network models population divergence and admixture. Existing methods for inferring population admixture network includes TreeMix (Pickrell and Pritchard, 2012).

The key feature of GTmix is that it works with local gene genealogies inferred from haplotypes. Existing methods for admixture network inference (e.g., Treemix) uses single-site genetic variations. While these methods are very fast, they may lose important linkage disequilibrum (LD) information. GTmix works with inferred local gene genealogies from haplotypes. Local genealogies contain LD information. GTmix performs maximum likelihood inference of admixture networks from local genealogies based on the so-called "multispecies coalescent" model. My tests show that GTmix is slower than TreeMix. However, GTmix can infer more accurate networks than TreeMix even when GTmix only uses a small fraction of data compared to those used by TreeMix.


There are two files needed to run GTmix. First, you need a file containing the local gene genealogies. Here is what is in the given example file (test-mat-2.hap.trees):

0       ((((2,6),8),(1,5)),((4,7),3))
1       ((((((1,7),3),(2,6)),5),8),4)
2       (((((1,5),7),(2,8)),6),(3,4))
3       (((((1,5),3),7),((6,8),2)),4)
4       (((((1,5),3),7),((6,8),2)),4)
5       (((((1,5),(2,4)),7),(6,8)),3)
6       ((((1,5),7),(2,4)),((6,8),3))
7       ((((1,5),2),((4,7),3)),(6,8))
8       (((((6,8),1),4),((2,5),7)),3)
9       ((((6,8),1),(3,7)),((2,5),4))
10      ((((2,5),4),7),(((6,8),1),3))

Note that the first column (index from 0 to 10) is optional; it is included here mainly because we use the program RENT+ to infer the gene genealogies; RENT+ outputs a tree index in its genealogy inference output. 

The second file you need is a file describing the sampled haplotypes and populations. For example, here is what is in the given example file (listPopInfo-p4-a2.txt):

A 2 1 2 
B 2 3 4 
C 2 5 6 
D 2 7 8

Here, each population is one row. The first column specifies the population name. The second column is the number of sampled haplotypes in the population. The following columns specify the haplotypes for this population (should match the taxa in the genealogies).

Once you have the executable for GTmix, simple type to run:

./gtmix -P listPopInfo-p4-a2.txt test-mat-2.hap.trees 

This would infer a population admixture network based on maximum likelihood. The inferred network is output (as a list of population trees contained in the network, and also as a graph in GML format, with default name being optimal-network.gml).

Refer to the user manual for more details on how to use GTmix.

# Preprocessing
The number of local genealogies inferred by RENT+ can be large. It may be necessary to choose a subset of local genealogies for inferring admixture networks. I recommend to use TreePicker I wrote for choosing a subset of trees. Check out the README file for more information on how to run RENT+ (Java exectuable included) and TreePicker.

# About source code release
I plan to release the source of GTmix sometime soon. For now, I provide executables for Mac and Linux (64 bits) for evaluation.

