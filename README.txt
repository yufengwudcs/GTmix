Instructions on how to use GTmix

First, I would suggest you to read the user manual of GTmix (in PDF). This document is for a quick start only.

******************************************************************************
BEFORE START

Running GTmix should be easy. However, to run GTmix, you need to have a set of haplotypes. To obtain haplotypes, you may need a phasing software tool, such as beagle. 

Moreover, for large data, GTmix can be slow. You may want to experiment with GTmix to see whether it is practical for your data.

******************************************************************************
Quick test

Executables are provided for Mac and Linux. Suppose you will use these pre-built executables. 

Now first take a look at the haplotypes. A small example is test-mat-1.hap. This file has four haplotypes, one for each of the four populations. 

First, run RENT+ (its executable is provided for your convenience) to infer local genealogies:

java -jar RentPlus.jar test-mat-1.hap

After this step, the inferred local genealogies are stored in test-mat-1.hap.trees. Note that the first line of the haplotypes contains the positions of the SNP sites. I would suggest to use fractional positions (between 0 and 1). 

Then, run TreePicker to choose a number of trees. Here, test-mat-1.hap.trees contains the list of local genealogies inferred by Rent+.

./treepicker-mac test-mat-1.hap.trees   test-mat-1.hap 5 > test-mat-1.hap.trees.chosen 

This should generate a file containing five chosen trees.

Finally, run GTmix:

./gtmix-mac -n 1  -P listPopInfo-p4-a1.txt test-mat-1.hap.trees.chosen  

Here, -n 1 means we are to infer a network with 1 admixture event. listPopInfo-p4-a1.txt specifies which haplotypes are from which population. One line for each populatioin. 

Now you can try the second example data, which has two haplotypes per population.

java -jar RentPlus.jar test-mat-2.hap
./treepicker-mac test-mat-2.hap.trees   test-mat-2.hap 5 > test-mat-2.hap.strees.chosen
./gtmix-mac -n 1  -P listPopInfo-p4-a2.txt test-mat-2.hap.trees.chosen

******************************************************************************
If you want to compile from the source, cd to the source code directory and type "make". This should build GTmix. 

You will also need to compile TreePicker. For this, change directory to the TreePicker. Then,
to compile TreePicker, type:

g++ Utils.cpp Utils2.cpp Utils3.cpp Utils4.cpp PhylogenyTreeBasic.cpp BinaryMatrix.cpp BioSequenceMatrix.cpp UnWeightedGraph.cpp  GTTreePicker.cpp -o treepicker

For some systems, you may need to add compile option:  -std=c++11

This should build TreePicker. I would recommend to copy TreePicker to the working directory. 


