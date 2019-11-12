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
## BWA ##

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


## BWA-SW ##

BWA-SW builds FM-indices for both the reference and query sequence. It
implicitly represents the reference sequence in a prefix trie and represents
the query sequence in a prefix directed acyclic word graph, which is transformed from the prefix trie of the query
sequence. A dynamic programming can be applied between
the trie and the DAWG, by traversing the reference prefix trie and the
query DAWG, respectively. This dynamic programming would find all local
matches if no heuristics were applied, but would be no faster than BWTSW.

The prefix trie of string X is a tree with each edge labeled with a symbol
such that the concatenation of symbols on the path from a leaf to the root
gives a unique prefix of X. The concatenation of edge symbols from a node
to the root is always a substring of X, called the string represented by the
node. The SA interval of a node is defined as the SA interval of the string
represented by the node. Different nodes may have an identical interval, but
recalling the definition of SA interval, we know that the strings represented
by these nodes must be the prefixes of the same string and have different
lengths.
The prefix DAWG, of X is transformed from the prefix trie by collapsing
nodes having an identical interval. Thus in the prefix DAWG, nodes and SA
intervals have an one-to-one relationship, and a node may represent multiple
substrings of X, falling in a sequence where each is a prefix of the next as is
discussed in the previous paragraph. Figure below gives an example.

![Figure 2](https://i.imgur.com/lYbygA5.png)
*(A) Prefix trie. Symbol ‘∧’ marks the start of a string. The two numbers in a node gives the
SA interval of the node. (B) Prefix DAWG constructed by collapsing nodes
with the identical SA interval. For example, in the prefix trie, three nodes
has SA interval [4,4]. Their parents have interval [1,2], [1,2] and [1,1],
respectively. In the prefix DAWG, the [4,4] node thus has parents [1,2] and
[1,1]. Node [4,4] represents three strings ‘OG’, ‘OGO’ and ‘OGOL’ with the
first two strings being the prefix of ‘OGOL’.*

It construct a prefix DAWG G(W) for the query sequence W and a prefix
trie T (X) for the reference X. The dynamic programming for calculating the
best score between W and X is as follows. Let Guv=Iuv=Duv=0 when u is
the root of G(W) and v the root of T (X). At a node u in G(W), for each of
its parent node u', calculate

![DynamicProgramming1](https://i.imgur.com/Z4jFUVb.png)

where v' is the parent of v in T (X), function S(u'
,u;v'
,v) gives the score
between the symbol on the edge (u'
,u) and the one on (v'
,v), and q and r
are gap open and gap extension penalties, respectively. G<sub>uv</sub>, I<sub>uv</sub> and D<sub>uv</sub> are
calculated with:

![DynamicProgramming12](https://i.imgur.com/RaKkwfw.png)

where pre(u) is the set of parent nodes of u. G<sub>uv</sub> equals the best score between
the (possibly multiple) substrings represented by u and the (one) substring
represented by v. We say a node v matches u if G<sub>uv</sub>>0.
The dynamic programming is performed by traversing both G(W) and
T(X) in the reverse post-order (i.e. all parent nodes are visited before
children) in a nested way. Noting that once u does not match v, u does not
match any nodes descending from v, we only need to visit the nodes close
to the root of T (X) without traversing the entire trie, which greatly reduces
the number of iterations in comparison to the standard Smith–Waterman
algorithm that always goes through the entire reference sequence.



---
# Script Submission #

You are required to submit your scripts to the shared __[Dropbox folder]()__ of this course. Your submission must include:

1. __Sufficient__ documentation on how to run the script 
2. Written comments that are __detailed__ enough to understand your scripts

## DEADLINE: November 18 ##


<center> <b> GOOD LUCK!! </b> </center>

