---
title: "JN_Bio720_Assignment4_Bioconductor"
author: "Jalees Nasir"
date: "December 4, 2018"
output: 
  html_document:
    keep_md: true
---

# Introduction to Bioconductor
# a) What is Bioconductor?
Introduction to the Bioconductor Project
-	Bioconductor has its own repository and way to install packages


```r
# source("https://bioconductor.org/biocLite.R")
# biocLite("packageName")
```
-	Bioconductor's BiocInstaller package has script biocLite()
-	biocLite() is used to install Bioconductor packages
-	Sourcing biocLite tells you about available updates

Remember, you can check the Bioconductor version using the function biocVersion() from BiocInstaller.


```r
# Load the BiocInstaller package
# library(BiocInstaller)

# Explicit syntax to check the Bioconductor version
# BiocInstaller::biocVersion()

# When BiocInstaller is loaded use biocVersion alone
# biocVersion()
```
BiocLite to install packages
The BSgenome package has been installed for you. This is a data package which contains representations of several genomes. 

```r
# source("https://bioconductor.org/biocLite.R")
# biocLite("BSgenome")
```
Any bioconductor package can be installed using the code above, then loaded using library(packageName). To check the package's version, use the function packageVersion("packageName").
The role of S4 in Bioconductor
S4 class definition
We will use the class BSgenome, which is already loaded for you.
Let's check the formal definition of this class by using the function showClass("className"). Check the BSgenome class results and find which are the parent classes (Extends) and the classes that inherit from it (Subclasses).

```r
# showClass("BSgenome")
```
Interaction with classes
Let's say we have an object called a_genome, from class BSgenome. With a_genome, you can ask questions like these: 
# Check its main class
class(a_genome)  # "BSgenome"

# Check its other classes
is(a_genome)  # "BSgenome", "GenomeDescription"

# Is it an S4 representation?
isS4(a_genome)  # TRUE
If you want to find out more about the a_genome object, you can use the accessor show(a_genome) or use other specific accessors, from the list of .S4methods(class = "BSgenome").
Remember you can use .S4methods() or showMethods() to check the accessors list of a class or a function.
Introducing biology of genomic datasets
Discovering the Yeast genome
Let's continue to explore the yeast genome using the package BSgenome.Scerevisiae.UCSC.sacCer3 which is already installed for you. In this exercise, you will load it and assign it to the object yeastGenome:
library(BSgenome.Scerevisiae.UCSC.sacCer3)
yeastGenome <- BSgenome.Scerevisiae.UCSC.sacCer3
As with other data in R, we can use head() and tail() to explore the yeastGenome object. We can also subset the genome by chromosome by using the $, in this way object_name$chromosome_name. If you need the names of the chromosomes use the names() function.
Another nifty function is nchar(), used to count the number of characters in a sequence.


```r
# Load the yeast genome
#library(BSgenome.Scerevisiae.UCSC.sacCer3)

# Assign data to the yeastGenome object
#yeastGenome <- BSgenome.Scerevisiae.UCSC.sacCer3

# Get the head of seqnames and tail of seqlengths for yeastGenome
#head(seqnames(yeastGenome))
#tail(seqlengths(yeastGenome))

# Select chromosome M, alias chrM
#yeastGenome$chrM

# Count characters of the chrM sequence
#nchar(yeastGenome$chrM)
```
Partitioning the Yeast genome
Genomes are often big, but interest lies in specific regions, so we need to subset a genome by extracting parts of it. To pick a sequence interval use getSeq() and specify the name of the chromosome, and the start and end of the sequence interval.

```r
#getSeq(yeastGenome, names = "chrI", start = 100, end = 150)
```
Notice that names is optional, if not specified it will return all chromosomes. Also the parameters start and end are optional, you could provide one or both, start my default will be 1 and end will be the length of the sequence.

```r
# Load the yeast genome
#library(BSgenome.Scerevisiae.UCSC.sacCer3)

# Assign data to the yeastGenome object
#yeastGenome <- BSgenome.Scerevisiae.UCSC.sacCer3

# Get the first 30 bases of each chromosome
#getSeq(yeastGenome, end = 30)
```
Available Genomes
As a recap, the BSgenome package makes available various public genomes. If you want to explore the available genomes from this package, you can use
available.genomes()
The list of names will appear in the following format: BSgenome.speciesName.provider.version. 





# b) Biostrings and when to use them?
Introduction to Biostrings
Exploring the Zika virus sequence
It's your turn to explore the Zika virus genome, named here as zikaVirus. The genome was downloaded from NCBI and you can apply Biostrings functions to learn more about it.
Start by checking the alphabet of the sequence.
alphabet() # shows the letters included in the sequence
# baseOnly = TRUE argument examines the alphabet corresponding to DNA or RNA bases

alphabetFrequency() # shows the counts per letter
Remember from the video that each alphabet corresponds to a specific biostring container, and each alphabet usually have extra code letters and symbols.
Manipulating Biostrings
Using a short sequence dna_seq from the zikaVirus object, it is your turn to have a go with the two biological processes of transcription and translation.
It is very exciting that at the end of the exercise you will be able to do this in two ways!
# Unlist the set and select the first 21 letters as dna_seq, then print it
dna_seq <- subseq(unlist(zikaVirus), end = 21)
dna_seq

# 1.1 Transcribe dna_seq as rna_seq, then print it
rna_seq <- RNAString(dna_seq) 
rna_seq

# 1.2 Translate rna_seq as aa_seq, then print it
aa_seq <- translate(rna_seq)
aa_seq

# 2.1 Translate dna_seq as aa_seq_2, then print it
aa_seq_2 <- translate(dna_seq)
aa_seq_2
Sequence handling
 

 
From a set to a single sequence
From the video, you know that sequences can be loaded into R using the function readDNAStringSet()
# Create zikv with one collated sequence using `zikaVirus`
zikv <- unlist(zikaVirus)

# Check the length of zikaVirus and zikv
length(zikaVirus)
length(zikv)

# Check the width of zikaVirus
width(zikaVirus)

# Subset zikv to only the first 30 bases
subZikv <- subseq(zikv, end = 30)
subZikv
Subsetting a set
In the previous exercise, you've used subseq() to subset a single sequence. Here, you can try subseq() using a Set with 3 sequences. The arguments are the same as before: object, start and end. The last two should be in the form of a vector, for each of the sequences on the set.
subseq(zikaSet, 
        start = c(20, 40, 2), 
        end = c(50, 45, 22)
     )
Common sequence manipulation functions
So far, you've learned the the most common sequence manipulation functions: 
`reverse()` 
`complement()`
`reverseComplement()`
`translate()`
In real life, you can manipulate really large sequences using these functions. 

Why are we interested in patterns?
Searching for a pattern
Let's find some occurrences of a pattern in the zikaVirus set using vmatchPattern(). Then, let's try the same pattern search using matchPattern() with a single sequence zikv.
# For Sets (1 to string, or vice-versa)
vmatchPattern(pattern = "ACATGGGCCTACCATGGGAG", 
              subject = zikaVirus, max.mismatch = 1)
# For single sequences
matchPattern(pattern = "ACATGGGCCTACCATGGGAG", 
              subject = zikv, max.mismatch = 1)

Finding Palindromes
It is your turn to find palindromic sequences using the zikv sequence. Remember findPalindromes() searches in a single sequence and does not work with a set.




# c) IRanges and GenomicRanges
IRanges and Genomic Structures
 
IRanges
Sequence analysis looks for particular sequences and their locations. As you've learned in the previous chapters, you can store sequences with their own alphabets and order, and you can also focus on certain intervals of these sequences. To extract sequence intervals you use ranges. The Bioconductor package IRanges comes in handy with its function IRanges(), which is a vector representation of a sequence range used to facilitate subsetting and annotation.
Constructing IRanges
Using the IRanges() function, you can specify some parameters, such as: start, end or width. These parameters can be numeric vectors, or you could set the start as a logical vector. Missing arguments will be resolved following the equation width = end - start + 1. 
The IRanges() constructor indicates that all of the parameters are optional with default NULL:
IRanges(start = NULL, end = NULL, width = NULL, names = NULL)
Interacting with IRanges
You can use the IRanges() function to create a single sequence:
# First example
(IRanges(start = 10, end = 37))
But also you can use vectors to create multiple sequence ranges at the same time. This is both fascinating and useful! 
# second example
(IRanges(start = c(5, 35, 50),
                  end = c(12, 39, 61),
             names = LETTERS[1:3]))
Remember width = end - start + 1.
Gene of Interest
GenomicRanges object from a data.frame
You can define a GRange with seqnames, start and end. If you have data in table format, you can also transform it into GRanges. Let's use a data.frame called df, as this is most likely where you have stored your sequence intervals. (Note: you can also use a tibble if you are more used to it)
The question is: What are the three ways to transform this df into a GRanges object?
The result will look like this:
GRanges object with 4 ranges and 0 metadata columns:
      seqnames    ranges strand
         <Rle> <IRanges>  <Rle>
  [1]     chrI  [11, 36]      *
  [2]     chrI  [12, 37]      *
  [3]    chrII  [13, 38]      *
  [4]    chrII  [14, 39]      *

Answer:
gr_df <- as(df, "GRanges")
gr_df <- GRanges(df)
gr_df <- as(df, "GenomicRanges")


GenomicRanges accessors
The following are some of the basic accessors for GRanges:
seqnames(gr)
ranges(gr)
mcols(gr)
genome(gr)
seqinfo(gr)
For a complete list of accessors, you can check methods(class = "GRanges").
 
Human genome chromosome X
 

Manipulating collections of GRanges
A sequence window
Sometimes you will want to split your GRanges. As you have seen in the video, GRanges provides the slidingWindows() function, where you specify arguments such as width and step.
slidingWindows(x, width, step = 1L)

GRangesLists also come with useful accessor functions; almost all the accessors from IRanges and GRanges are reused, but the result will be a list instead. You can find accessors using the function methods(class = "GRangesList").
How many transcripts?
How many transcripts does ABCD1 has. You can find out by using:

transcriptsBy(x, by = "gene")

 


```r
# load the human transcripts DB to hg
#library(TxDb.Hsapiens.UCSC.hg38.knownGene)
# hg <- TxDb.Hsapiens.UCSC.hg38.knownGene

# prefilter chromosome X
#seqlevels(hg) <- c("chrX")

# get all transcripts by gene and print it
#hg_chrXt <- transcriptsBy(hg, by = "gene")
#hg_chrXt

# select gene `215` from the transcripts
#hg_chrXt$'215'
```

# d) Introducing ShortRead
Sequence files
Reading in files
You have learned that readFasta() and readFastq(), both need a location and a file pattern to read one or many files.

 
 
Sequence quality

 
>=30 ??? high quality

 
 
Exploring sequence quality
To check the encoding values for each letter in quality(), use encoding():
encoding(quality(fqsample))
For a quality assessment (QA) summary use qa():
qaSummary <- qa(fqsample, type = "fastq", lane = 1)
QA elements can be accessed with qaSummary[["nameElement"]]). You can also have a look at a web report using the function browseURL(report(qaSummary))

```r
# load ShortRead
#library(ShortRead)

# Check quality
#quality(fqsample)

# Check encoding
#encoding(quality(fqsample))

# Check baseQuality
#qaSummary[["baseQuality"]]
```

Try your own nucleotide frequency plot
Now it's time to take a closer look at the frequency of nucleotides per cycle. The best way to do this is by making a visualization. Usually the first cycles are a bit random and then the frequency of nucleotides should stabilize with the coming cycles.
This exercise uses the complete fastq file SRR1971253 with some pre processing done for you.
library(ShortRead)
fqsample <- readFastq(dirPath = "data", 
                      pattern = "SRR1971253.fastq")
abc <- alphabetByCycle(sread(fqsample))
# transpose nucleotides A, C, G, T per column
nucByCycle <- t(abc[1:4,]) 
# tidy dataset
nucByCycle <- nucByCycle %>% 
  as.tibble() %>% # convert to tibble
  mutate(cycle = 1:50) # add cycle numbers

To plot:
# glimpse nucByCycle
glimpse(nucByCycle)

# make an awesome plot!
nucByCycle %>% 
  # gather the nucleotide letters in alphabet and get a new count column
  gather(key = alphabet, value = count , -cycle) %>% 
  ggplot(aes(x = cycle, y = count, color = alphabet)) +
  geom_line(size = 0.5 ) +
  labs(y = "Frequency") +
  theme_bw() +
  theme(panel.grid.major.x = element_blank())
Match and filter
 

 
Filtering reads on the go!
What if, from all of the reads in a file, you are only interested in some of those reads? You can use a filter!
Let's say that you are interested only in those reads that start with the pattern "ATGCA". A tiny filtering function can do the job, making use of the srFilter() function.
myStartFilter <- srFilter(function(x) substr(sread(x), 1, 5) == "ATGCA")

# Load package ShortRead
library(ShortRead)

# Check class of fqsample
class(fqsample)

# filter reads into selectedReads using myStartFilter
selectedReads <- fqsample[myStartFilter(fqsample)]

# Check class of selectedReads
class(selectedReads)

# Check detail of selectedReads
detail(selectedReads)

More filtering!
Awesome, now that you are all about filtering reads, let's pick the tiny function that removes reads with a maximum of 2 Ns per read. 
myNFilt <- nFilter(threshold = 2)
Multiple assessment

 
 
Plotting cycle average quality
# Load package Rqc
library("Rqc")

# Average per cycle quality plot
rqcCycleAverageQualityPlot(qa)

# Average per cycle quality plot with white background
rqcCycleAverageQualityPlot(qa) + theme_minimal()

# Read quality plot with white background
rqcReadQualityPlot(qa) + theme_minimal()


