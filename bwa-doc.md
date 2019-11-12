## CS4255 2019 Project Instructions for BWA and BWA-SW ##

# Problem statement #
BWA and BWA-SW are tools for aligning short (less than 100 bp) and long (less than 1 Mbp) reads to a large reference. 

### Script 1 ###

__Input:__  Large reference and set of short reads.

__Output:__ Alignment of the short reads and the reference.

---

# Testing your code #

## Test data ##


---

### Script 2###

__Input:__  Large reference and set of long reads.

__Output:__ Alignment of the long reads and the reference.

# Testing your code #

## Test data ##
---

# Some useful notes  #
## BWA
To find inexact matches of string W (ends with A/C/G/T) in reference X (ends with $) the following steps need to be taken:

1- Calculate BWT string for reference string X

*Note: You can use the BWT implementation for excercise 33*

2- Calculate array C(.) and O(.,.) from B

*Note: C(a) is the number of symbols in X[0, n-2] that are lexicographically smaller than a and O(a,i) is the number of occurance of a in B[0,i]*

3- Calculate BWT string B' for the reverse reference

4- Calculate array O'(.,.) from B'

You can then calculate inexact matches of string W with referece X with no more than z differences (missmatches or gaps) by Following [Algorithm 1].

![Algorithm 1](https://i.imgur.com/tbqGFMo.png) 
 
Algorithm 1 gives a recursive algorithm to search for the SA intervals of
substrings of X that match the query stringW with no more than z differences
(mismatches or gaps). Essentially, this algorithm uses backward search to
sample distinct substrings from the genome. This process is bounded by the
D(·) array where D(i) is the lower bound of the number of differences in
W[0,i]. The better the D is estimated, the smaller the search space and the
more efficient the algorithm is.Anaive bound is achieved by setting D(i)=0 for all i, but the resulting algorithm is clearly exponential in the number of
differences and would be less efficient.

The CalculateD procedure in [Algorithm 1] gives a better, though not optimal,
bound. It use the BWT of the reverse (not complemented)
reference sequence to test if a substring of W is also a substring of X.

To understand the role of D, look at example of searching
for W =LOL in X=GOOGOL$ [Figure 1]. If you set D(i)=0 for all i and
disallow gaps (removing the two star lines in the [Algorithm 1]), the call graph
of InexRecur, which is a tree, effectively mimics the search route shown
as the dashed line in [Figure 1]. However, with CalculateD, you know that
D(0)=0 and D(1)=D(2)=1.We can then avoid descending into the ‘G’ and
‘O’ subtrees in the prefix trie to get a much smaller search space.

![Figure 1](https://i.imgur.com/wVpLecs.png)


## BWA-SW
 
---
# Script Submission #

You are required to submit your scripts to the shared __[Dropbox folder]()__ of this course. Your submission must include:

1. __Sufficient__ documentation on how to run the script 
2. Written comments that are __detailed__ enough to understand your scripts

## DEADLINE: November 18 ##


<center> <b> GOOD LUCK!! </b> </center>

