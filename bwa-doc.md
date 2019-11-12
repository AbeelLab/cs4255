## CS4255 2019 Project Instructions for BWA and BWA-SW ##

# Problem statement #
BWA and BWA-SW are tools for aligning short (less than 100 bp) and long (less than 1 Mbp) reads to a large reference. 

### Script 1###

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
For implementing BWA aligner following steps need to be taken
1- Calculate BWT string for reference string X
*Note: You can use the BWT implementation for excercise 33*
2- Calculate array C(.) and O(.,.) from B
*Note:*
3- Calculate BWT string B' for the reverse reference
4- Calculate array O'(.,.) from B'

You can then calculate inexact matches of string W with the referece X with no more than z differences (missmatches or gaps).

![Algorithm](https://ibb.co/7jxcsht) 


## BWA-SW
 
---
# Script Submission #

You are required to submit your scripts to the shared __[Dropbox folder]()__ of this course. Your submission must include:

1. __Sufficient__ documentation on how to run the script 
2. Written comments that are __detailed__ enough to understand your scripts

## DEADLINE: November 18 ##


<center> <b> GOOD LUCK!! </b> </center>

