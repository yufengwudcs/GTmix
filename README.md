# GTmix
Inference of population admixture network from local gene genealogies under coalescent theory

GTmix is a computer program for inferring population admixture networks from inferred local gene genealogies. If you use GTmix, please cite:

Inference of Population Admixture Network from Local Gene Genealogies: a Coalescent-based Maximum Likelihood Approach, Yufeng Wu, Bioinformatics, Volume 36, Issue Supplement_1, July 2020, Pages i326–i334, https://doi.org/10.1093/bioinformatics/btaa465.

Population admixture network is a model for population evolution. Admixture network models population divergence and admixture. Existing methods for inferring population admixture network includes TreeMix (Pickrell and Pritchard, 2012).

The key feature of GTmix is that it works with local gene genealogies inferred from haplotypes. Existing methods for admixture network inference (e.g., Treemix) uses single-site genetic variations. While these methods are very fast, they may lose important linkage disequilibrum (LD) information. GTmix works with inferred local gene genealogies from haplotypes. Local genealogies contain LD information. GTmix performs maximum likelihood inference of admixture networks from local genealogies based on the so-called "multispecies coalescent" model. My tests show that GTmix is slower than TreeMix. However, GTmix can infer more accurate networks than TreeMix even when GTmix only uses a small fraction of data compared to those used by TreeMix.

## Input files
There are two files needed to run GTmix. First, you need a file containing the local gene genealogies. Here is what is in the given example file (test-mat-2.hap.trees). BTW, Don't leave empty rows in input files.

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

### Preprocessing
The number of local genealogies inferred by RENT+ can be large. It may be necessary to choose a subset of local genealogies for inferring admixture networks. I recommend to use TreePicker I wrote for choosing a subset of trees. Check out the README file for more information on how to run RENT+ (Java exectuable included) and TreePicker.

## Source code release (Jan. 13, 2022)
The source code of GTmix has been released (file: GTmix-ver1.3.0.5-src.tar). To build GTmix from source, you only need to (i) decompress the source code, and (ii) type "make" under the source code directory. That should be all you need (assuming your computer is not too old). GTmix has literally no dependency.

## Tutorial on running GTmix on simulated data
Now that you have got GTmix to run on the provided data, I guess you may want to try it with real genetic data. I highly recommend you to first experiment with simulated data first. To help you get started with GTmix, I have prepared the following tutorial.
### Things you need for the tutorial
- Download the tutorial file and decompress it and cd to this directory
- Create a link (or copy the executable) of GTmix in the tutorial directory
- Create a link to the TreePicker utility in the tutorial directory (that works for your computer). You can build the executable if you want or just copy the proper executable (use the right OS pre-built version; for me, it is Mac).
- Use sh/bash shell in your command line environment

### Automated executition
You can now try to run GTmix on the provided haplotypes by simply typing (you may need to use chmod +x gtmix-tuotiral.sh first):

./gtmix-tuotiral.sh

You should expect the script starts to run and dump out informative lines such as:

STEP ONE: infer local trees from haplotypes\
Data 1\
Data 2\
..\
STEP TWO: pick local trees from each locus to form the tree files to GTmix\
Data 1\
Data 2\
..\
STEP THREE: finally run GTmix\
*** GTMix ver. 1.3.0.5, October 30, 2019 ***\

Number of gene trees to use: 50\
Scaffold tree: (B:1.770781,(A:3.723438,(E:4.211563,(C:3.716250,D:3.661250):0.248437):0.139688):1.770781)\
****** Highest log-probabiliyt of optimized network (searching over network space): -4688.49\
Time needed to find the optimal network: 112\
Optimal network: output to file: optimal-network.gml\
List of marginal trees in the optimal network:\
[0.432092] (E:0.641165,((C:0.194904,D:0.189140):0.090975,(A:0.141690,B:0.086315):0.138844):0.275980)\
[0.567908] (E:0.641165,(B:0.181085,(A:0.280533,(C:0.194904,D:0.189140):0.090975):0.105796):0.170184)\
Admixture population: B\
Elapsed time = 112 seconds.\

If the shell script breaks down, you should try to fix the issues now. One way is to try to run the script manually. To do that, take a look at the script and then run the parts of the script manually. 
### Explanations
Let us try to make sense about what the tutorial is about. 
#### Data
The data in 20x50x50x50-4p4a-og-t0.1.net3.1.trace was simulated using Hudson's ms. Take a look at the first row of this file:  
./ms 20 50 -t 50 -r 50 500000 -I 5 4 4 4 4 4 -es 0.02 2 0.5 -ej 0.04 2 1 -ej 0.06 4 3 -ej 0.08 3 1 -ej 0.10 6 1 -ej 0.20 5 1  
I assume you have used ms before. Otherwise you will need to read the manual of ms to make sense of this command line. Briefly, we are generating haplotypes for 50 loci, each with 20 haplotypes; there are five populations, each with four haplotypes. The second population (population 2) is admixed; one ancestral population  of 2 and population 1 split from an ancestral population (G), which splits from the ancestral population (H) of populations 3 and 4 (which are children of H) at an ancestral population I; finally, the second ancestral population of 2 splits from I; E is the outgroup. This is shown in this figure. 
![Simulated admixture network](https://github.com/yufengwudcs/GTmix/edit/master/AdmixNet3-github-tutorial.png?raw=true)
