## CS4255 2019 Project Instructions for Minimap and Miniasm ##

# Problem statement #

You are given long read sequences of _E. coli_ in FASTQ format as well as short reads; in this project you are asked to implement an assembly pipeline comprising as described below.

### Script ###
Implement the minimap algorithm for read alignment, and miniasm algorithm for assembling the genome graph. 

__Input:__ Long reads in FASTQ format <br/>
__Output:__ Genome assembly in FASTA format

---

# Testing your code #

## Test data ##

### 1. You can test your codes on the [small test dataset with 3 reads](https://github.com/AbeelLab/cs4255/blob/master/minimap-reallysmall.fastq) ###
*Use k = 10 and w = 3* 

Correct output (using the same notation as in *Algorithm 4* from the paper:
```python
{0: [((0, 0, 0, 0), (0, 0, 0, 20))], 2: [((2, 1, 12, 6), (2, 1, 28, 14))]}
{1: [((1, 0, 0, 1), (1, 0, 0, 19))]}
{0: [((0, 1, 12, 6), (0, 1, 28, 14))], 2: [((2, 0, 0, 0), (2, 0, 0, 20))]}
```
I.e., `(0, 0, 0, 4)` = (*t*, *r*, *c*, *i'*) which translates to (read #, strand, distance, position), see [*Algorithm 4*](https://i.imgur.com/01keMVE.gif)

### 2. A bigger sample test is also [on the github page here](https://github.com/AbeelLab/cs4255/blob/master/minimap-sample.fastq) ###

## Intermediate tests ##

### 1. Hash function ###
Your script should <br/>
(1) convert the string input `TTA` to the number `60` <br/>
(2) convert the number `60` to `9473421487448830983`<br/>

### 2. Finding minimizers ###
For sequence input `"GATTACAACT"`, *w* = 4 and *k* = 3 your script should find the minimizers:
```python
(1396078460937419741, 6, 1)
(2859083004982788208, 3, 1)
```

---

# Some useful notes  #

Your script should consist of __2 main functions__: first one for aligning with minimap and the second one for assembling with miniasm. Here is a quick recap of both methods from the [corresponding paper](https://academic.oup.com/bioinformatics/article/32/14/2103/1742895).

## 1. Minimap ##

 1.1. Computing minimizers
 >Recall: a (*w*, *k*) minimizer of a string *s* is the smallest kmer in a surrounding window of _w_ consecutive kmers. 
>
> ![Algorithm 1](https://i.imgur.com/azgZ4zR.gif) ![Algorithm 2](https://i.imgur.com/C1RwrPX.gif)
 
 1.2. Indexing
 > Using the output from _Algorithm 1_ you should make a hash table for minimizers of all target sequences:  Key is the minimizer hash and the value is a set of target sequence index, the position of the minimizer and the strand.
>
> ![Algoirthm 3](https://i.imgur.com/1qfoNEH.gif)

_Hint: When you are implementing, you should append minimizers to an array (includes minimizer sequence and its position), and then sort this array once you have collected all the minimizers._

 1.3. Mapping
>Collect all the minimizer hits between the query and all the target sequences. Then, perform single-linkage clustering to group approximately colinear hits. 
>Recall: Some hits in a cluster may not be colinear because two minimizer hits within distance ϵ are always ϵ-away. To fix this issue, you should find the maximal colinear subset of hits by solving a longest increasing sequencing problem (see line 3 in _Algorithm 4_)
>
>![Algorithm 4](https://i.imgur.com/01keMVE.gif)

_HINT: In implementation, set thresholds on the size of the subset (size is 4 by default) and the number of matching bases in the subset to filter poor mappings (100 for read overlapping)._

_AN EVEN BETTER HINT: To fin the "maximal colinear subset", instead of implementing the algorithm from scratch you can [use this answer on stackoverflow](https://stackoverflow.com/questions/3992697/longest-increasing-subsequence)_

## 2. Miniasm ##

> Recall: assembly graphs are directed acyclic graphs whose vertices are substrings, and the edges between two vertices represent their overlap relationship.
> 

2.1. Trimming reads
First things first, you should trim your reads by examining the read-to-read mappings. 
>For each read, you will compute per-base coverage based on good mappings against other reads (longer than 2000 bp with at least 100 bp non-redundant bases on matching minimizers). Then you should identify the longest region having coverage three or more, and trim bases outside this region.

2.2. Generating assembly graph <br/>
<i>HINT: There are 2 ways we can have two strings v and w mapped to each other based on their sequence similarity: (i) v is mapped to a substring of w if w contains v (ii) v overlaps w if a suffix of v and a prefix of w are mapped to each other (<b>written as v &rarr; w</b>)</i>

![Fig 1](https://i.imgur.com/lrE7aao.gif)

![Algorithm 5](https://i.imgur.com/faoOWtT.gif)

2.3. Graph cleaning <br/>
Once you complete steps 2.1 and 2.2 you should have an assembly graph. Now, you will be cleaning your graph by (i) removing _transitive_ edges (ii) trimming _tipping unitigs_ composed of few reads (4 by default in the paper) and (iii) popping small _bubbles_. 

> Recall: In an assembly graph, an edge *v* &rarr; *w* is **transitive** if there exsits *v* &rarr; *u* and *u* &rarr; *w*. A vertex *v* is a **tip** if deg<sub>*v*</sub><sup>+</sup> = 0 and deg<sub>*v*</sub><sup>-</sup> > 0. A **bubble** is a directed acyclic subgraph with a single source *v* and a single sink *w* having at least two paths between *v* and *w*, and without connecting the rest of the graph. The bubble is **tight** if deg<sub>*v*</sub><sup>+</sup> > 1 and deg<sub>*v*</sub><sup>-</sup> > 1
> 
>![Algorithm 6](https://i.imgur.com/WOqOvaw.gif)

2.4. Generating unitig sequences <br/>
Intuitively, a unitig is a maximal path on which adjacent vertices can be _unambiguously merged_ without affecting the connectivity of the original assembly graph.

**Now, you have two options for writing your script:**

(1) Implement both minimap and miniasm _from scratch_ OR <br/>
(2) Implement minimap from scratch only, and use the _miniasm tool_ provided by the authors

_HINT: If you choose to take the second route, you have to **modify your first function** to generate a **PAF file** as output because the miniasm tool only accepts PAF as input for assembly. See [author's webpage for more details on PAF](https://lh3.github.io/minimap2/minimap2.html#10)_
 
 ---
# Script Submission #

You are required to submit your scripts to the shared __[Dropbox folder]()__ of this course. Your submission must include:

1. __Sufficient__ documentation on how to run the script 
2. Written comments that are __detailed__ enough to understand your scripts



<center> <b> GOOD LUCK!! </b> </center>

