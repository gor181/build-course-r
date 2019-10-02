---
title: 'IRanges and GenomicRanges'
description: 'The IRanges and GenomicRanges packages are also containers for storing and manipulating genomic intervals  and variables defined along a genome.  These packages provide infrastructure and support to many other Bioconductor packages because of their enriching features. You will learn how to use these containers and their associated metadata, for manipulation of your sequences. The dataset you will be looking at is a special gene of interest in the human genome.'
attachments:
    slides_link: 'https://s3.amazonaws.com/assets.datacamp.com/production/course_6307/slides/chapter3.pdf'
---

## IRanges and Genomic Structures

```yaml
type: VideoExercise
key: e2e376df75
lang: r
xp: 50
skills: 1
```

`@projector_key`
a50efebf5a44200f29b01a9bc547b930

---

## IRanges

```yaml
type: PureMultipleChoiceExercise
key: c6f4b3aad2
xp: 50
skills: 1
```

Sequence analysis looks for particular sequences and their locations. As you've learned in the previous chapters, you can store sequences with their own alphabets and order, and you can also focus on certain intervals of these sequences. To extract sequence intervals you use ranges. The Bioconductor package `IRanges` comes in handy with its function `IRanges()`, which is a vector representation of a sequence range used to facilitate subsetting and annotation.

Fill in the blank:

IRanges objects can be defined using two data types: ____ or ____ vectors.

`@hint`
Think of two different data types, hence you can also discard those that are subtypes.

`@possible_answers`
- numeric, integer
- integer, logical
- [logical, numeric]
- character, logical

`@feedback`
- 1) `integer` is a numeric, so that wouldn't be two data types.
- 2) since `integer` is a subset of numeric, this is not the complete answer.
- 3) Exactly! `IRanges` objects can be defined using either by `numeric` or `logical` vectors. Numeric vectors will delimit the exact positions by index, and logical vectors will select a range by a condition.
- 4) `characters` do not define ranges in `IRanges`, but are use to name your ranges.

---

## Constructing IRanges

```yaml
type: NormalExercise
key: 0d603a7864
lang: r
xp: 100
skills: 1
```

In the video, some IRanges constructor examples were provided. This is your turn to practice creating sequence ranges with different arguments, and see how these arguments are reused or complemented. 

Using the `IRanges()` function, you can specify the start, end or start, width as numeric vectors, or only the start as a logical vector. Missing arguments will be resolved following the equation `width = end - start + 1`. 

The `IRanges()` constructor
```{r}
IRanges(start = NULL, end = NULL, width = NULL, names = NULL)
```

`@instructions`
Construct an IRanges object with the following arguments
- `start` vector 1 to 5 and `end` 100 
- `end` 100 and `width` 89 and 10
- `start = Rle(c(F, T, T, T, F, T, T, T))`
- print the objects and see the results!

`@hint`
- You can use `:` or `c` for creating vectors.
- `T` is a shortcut of `TRUE` as `F` is of `FALSE`

`@pre_exercise_code`
```{r}
library(IRanges)
```

`@sample_code`
```{r}
# load package IRanges
library(___)

# start vector 1 to 5 and end 100 
IRnum1 <- ____

# end 100 and width 89 and 10
IRnum2 <- ____

# logical argument start = Rle(c(F, T, T, T, F, T, T, T))
IRlog1 <- IRanges(___ = Rle(___))

# Printing objects in a list
print(list(IRnum1 = IRnum1, IRnum2 = IRnum2, IRlog1 = IRlog1))
```

`@solution`
```{r}
# load package IRanges
library(IRanges)

# start vector 1 to 5, end 100 
IRnum1 <- IRanges(start = 1:5, end = 100)

# end 100, width 89 and 10
IRnum2 <- IRanges(end = 100, width = c(89, 10))

# logical argument start = Rle(c(F, T, T, T, F, T, T, T))
IRlog1 <- IRanges(start = Rle(c(F, T, T, T, F, T, T, T)))

# Printing objects in a list
print(list(IRnum1 = IRnum1, IRnum2 = IRnum2, IRlog1 = IRlog1))
```

`@sct`
```{r}
ex() %>% check_library("IRanges")
ex() %>% check_correct(
	check_object(.,"IRnum1") %>% check_equal(),
  	check_function(.,"IRanges", index=1) %>% {
      check_arg(.,"start") %>% check_equal()
      check_arg(.,"end") %>% check_equal()
    }
)
ex() %>% check_correct(
	check_object(.,"IRnum2") %>% check_equal(),
  	check_function(.,"IRanges", index=2) %>% {
      check_arg(.,"end") %>% check_equal()
      check_arg(.,"width") %>% check_equal()
    }
)
ex() %>% check_correct(
	check_object(.,"IRlog1") %>% check_equal(),
  	check_function(.,"IRanges", index=3) %>% {
      check_arg(.,"start") %>% check_equal(eval=FALSE)
    }
)

success_msg("Well done! you have created three `IRanges` objects using different arguments! This is useful to extract sequences on specific sections of your big sequence.")
```

---

## Interacting with IRanges

```yaml
type: MultipleChoiceExercise
key: bfcdf8b1e7
lang: r
xp: 50
skills: 1
```

You can use the `IRanges()` function to create a single sequence

```{r}
# First example
(IRanges(start = 10, end = 37))
```

But also you can use vectors to create multiple sequence ranges at the same time. This is both fascinating and useful! 

```{r}
# second example
(IRanges(start = c(5, 35, 50),
                  end = c(12, 39, 61),
             names = LETTERS[1:3]))
```

Remember `width = end - start + 1`

For this exercise, check the lengths of each of the examples provided here. Use either `width()` or `lengths()` functions with each of the examples and see what the difference is before selecting an answer .

`@possible_answers`
- [First example 28; second example 8, 5, 12]
- First example 27, second example 7, 4, 11
- First example 29, second example 9, 6, 13

`@hint`
- use the code snippets as input of `width()` or `lengths()`
- notice the difference between `length()` and `lengths()`

`@pre_exercise_code`
```{r}
library(IRanges)
```

`@sct`
```{r}
success_msg("Awesome! You are now able to create an use `IRanges` and check for their lengths or widths. This is going to be very useful when you want to select let's say a 100 base-pair rage from your big sequence. Well done!")
```

---

## Gene of Interest

```yaml
type: VideoExercise
key: 441b35cdb2
lang: r
xp: 50
skills: 1
```

`@projector_key`
78dde8b7ec545c0d53a59c2da1c99aae

---

## GenomicRanges object from a data.frame

```yaml
type: PureMultipleChoiceExercise
key: e542d27aea
xp: 50
```

In the video you learned ways to create `GRanges` objects. You can define `seqnames`, `start` and `end`. You can also transform data previously stored. Let's use a `data.frame` called `df`, as this is most likely where you have stored your sequence intervals. (Note: you can use the data structure you are more familiar with, like: `tibble` or `DataTable`) 

How can you transform this `df` into a `GRanges` object? For example:
```
GRanges object with 4 ranges and 0 metadata columns:
      seqnames    ranges strand
         <Rle> <IRanges>  <Rle>
  [1]     chrI  [11, 36]      *
  [2]     chrI  [12, 37]      *
  [3]    chrII  [13, 38]      *
  [4]    chrII  [14, 39]      *
```

`@hint`
`GRanges` and `GenomicRanges` are equivalent for this transformation.

`@possible_answers`
- [All code options below ]
- `gr_df <- as(df, "GRanges")`
- `gr_df <- GRanges(df)`
- `gr_df <- as(df, "GenomicRanges")`
- None of the options above

`@feedback`
- Very nice! you can now start creating `GRanges` objects from your existing datasets. If you are familiar with `tibbles`, the same can be applied, you can use your `tibbles` instead of `data.frames`
- 2) That's great! you are reinforcing what you learned from the video, but there are other 2 ways to transform `df`.
- 3) That's great! , but there are other 2 ways to transform `df`.
- 4) That's great! , but there are other 2 ways to transform `df`.
- 5) Not quite the right answer, there are 3 ways to transform `df` in this exercise.

---

## GenomicRanges accessors

```yaml
type: NormalExercise
key: def335d378
lang: r
xp: 100
skills: 1
```

In the previous exercise, you created a `GRanges` object from a `data.frame` with the basic information. You will discover that `GRanges` can 
store much more! 

Use the `GRanges` object called `gr` to investigate more of it using accessors
methods. You can check the `GRanges` object's characteristics like the name of each chromosome, the number of sequences, the names
of each sequence, information about strand, score, length and more.

The following are some of the basic accessors for `GRanges`

```{r}
seqnames(gr)
ranges(gr)
mcols(gr)
genome(gr)
seqinfo(gr)
```

For a complete list of accessors, you can check `methods(class = "GRanges")`

Use the object `myGR` for this exercise

`@instructions`
- Get sequence information from `myGR`
- Now check the metadata using `mcols()`

`@hint`
- With the object `myGR` use the `mcols()` function to check metadata.

`@pre_exercise_code`
```{r}
library(GenomicRanges)

# Load the Genomic Ranges object myGR
load("/usr/local/share/datasets/myGR.RData")
```

`@sample_code`
```{r}
# Load Package Genomic Ranges
library(___)

# Print the GRanges object
___

# Check the metadata, if any
___
```

`@solution`
```{r}
# Load Package Genomic Ranges
library(GenomicRanges)

# print the GRanges object
myGR

# Check the metadata, if any
mcols(myGR)
```

`@sct`
```{r}
ex() %>% check_library("GenomicRanges")
ex() %>% check_output_expr("myGR")
ex() %>% check_function("mcols") %>% check_arg("x") %>% check_equal()


success_msg("Good job!, Remember to check `methods(class = \"GRanges\")` if you want to find and use other accessors.")
```

---

## ABCD1 mutation

```yaml
type: PureMultipleChoiceExercise
key: 4145f9ab3f
xp: 50
skills: 1
```

You have just learned about the gene [ABCD1](https://ghr.nlm.nih.gov/gene/ABCD1#conditions). It encodes a protein in charge of the normal transport of a group of fats that keep brain and lung cells, in mammals, functioning normally. When these groups of fats are not broken down, they build up in the body and become toxic. This affects the adrenal glands (small glands on top of each kidney) and the insulation (myelin) that surrounds neurons, causing hormonal problems, deteriorating movement, vision and hearing. More than 650 mutations in the ABCD1 gene have been found to cause [X-linked adrenoleukodystrophy](https://rarediseases.info.nih.gov/diseases/5758/x-linked-adrenoleukodystrophy), a rare genetic disease. 

In which chromosome can we find the gene [ABCD1](http://www.ensembl.org/Homo_sapiens/Location/View?db=core;g=ENSG00000101986;r=X:153724868-153744762)?

`@hint`
The ABCD1 gene is located in a sex chromosome

`@possible_answers`
- [Chromosome X]
- Chromosome Y
- Chromosome 2
- Chromosome 23

`@feedback`
- Excellent! the ABCD1 gene is located in the long arm of the X chromosome at a position known as Xq28.
- The ABCD1 gene is located in a sex chromosome but not the Y 
- That's a bit far from the real location
- There is no chromosome named 23

---

## Human genome chromosome X

```yaml
type: NormalExercise
key: 7e8c41bd0a
lang: r
xp: 100
skills: 1
```

It is your turn to use the `TxDb.Hsapiens.UCSC.hg38.knownGene` package and extract information from it. For now, you will continue using the function `genes()` using as `filter` `tx_chrom = "chrX"`, as in the video. Remember that filter receives a `list()` of filter conditions.

The `filter` argument receives a *named list of vectors*, to restrict the output. Valid names for this list are: `"gene_id", "tx_id", "tx_name", "tx_chrom", "tx_strand", "exon_id", "exon_name", "exon_chrom", "exon_strand", "cds_id", "cds_name", "cds_chrom", "cds_strand" and "exon_rank"`

Use the `hg` object.

`@instructions`
- Load `TxDb.Hsapiens.UCSC.hg38.knownGene` using `library()`, then assign it to `hg` and print it.
- Extract all the genes in chromosome X `hg_chrXg`, then print it.
- Extract all positive stranded genes in chromosome X.
- Print sorted hg_chrXgp.

`@hint`
use the function `genes()` with `hg`. For the first part use the filter `tx_chrom = "chrX"`
For the second part you need to use `tx_strand = "+"`

`@pre_exercise_code`
```{r}
# needs to be loaded to use TxDb
library(GenomicFeatures)
```

`@sample_code`
```{r}
# load human reference genome hg38
library(___)

# assign hg38 to hg, then print it
___ <- TxDb.Hsapiens.UCSC.hg38.knownGene
hg

# extract all the genes in chromosome X as hg_chrXg, then print it
hg_chrXg <- ___
hg_chrXg

# extract all positive stranded genes in chromosome X as hg_chrXgp, then sort it
hg_chrXgp <- ___(hg, ___ = ___(tx_chrom = c("chrX"), tx_strand = "+"))
sort(___)
```

`@solution`
```{r}
# load human reference genome hg38
library(TxDb.Hsapiens.UCSC.hg38.knownGene)

# assign hg38 to hg, then print it
hg <- TxDb.Hsapiens.UCSC.hg38.knownGene
hg

# extract all the genes in chromosome X as hg_chrXg, then print it
hg_chrXg <- genes(hg, filter = list(tx_chrom = c("chrX")))
hg_chrXg

# extract all positive stranded genes in chromosome X as hg_chrXgp, then sort it
hg_chrXgp <- genes(hg, filter = list(tx_chrom = c("chrX"), tx_strand = "+"))
sort(hg_chrXgp)
```

`@sct`
```{r}
ex() %>% check_library("TxDb.Hsapiens.UCSC.hg38.knownGene")
ex() %>% check_object(.,"hg")
ex() %>% check_correct(
  	check_object(.,"hg_chrXg") %>% check_equal(eq_condition = "equivalent"),
	check_function(.,"genes", index=1) %>% {
      check_arg(.,"filter") %>% check_equal()
      check_arg(.,"x") %>% check_equal()
    }
)
ex() %>% check_correct(
  	check_object(.,"hg_chrXgp") %>% check_equal(eq_condition = "equivalent"),
	check_function(.,"genes", index=2) %>% {
      check_arg(.,"filter") %>% check_equal()
      check_arg(.,"x") %>% check_equal()
      check_arg(.,"tx_strand") %>% check_equal(eval=FALSE)
    }
)
ex() %>% check_function("sort") %>% check_arg("x") %>% check_equal()

success_msg("You really demonstrated good use of the extracting function `genes()`! Similar functions to explore are `transcripts()`, `cds()`, `promoters()`")
```

---

## Manipulating collections of GRanges

```yaml
type: VideoExercise
key: bbd9d0016c
lang: r
xp: 50
skills: 1
```

`@projector_key`
ff89c86f72bbafe4b5355d3eaae90f8d

---

## A sequence window

```yaml
type: MultipleChoiceExercise
key: 5a012f235e
lang: r
xp: 50
skills: 1
```

Sometimes you will want to split your GRanges. As you have seen in the video, `GRanges` provides the
`slidingWindows()` function, where you specify arguments such as `width` and `step`.

```
slidingWindows(x, width, step = 1L)
```

The `GRanges` object called `ABCD1`, with gene id 215 of length `19895`, has been pre-loaded for this exercise. Check the `ranges()` function to see its structure.

```
ranges(ABCD1)
```

If you wanted exactly 2 windows using `step = 1L`, what will be the maximum number allowed for the width of a window?

`@possible_answers`
- The length of the genome
- [The length of the GRanges object -1]
- The length of the GRanges object
- Any length lower

`@hint`
If you use exactly the same width of the gene, then you will only get one window.

`@pre_exercise_code`
```{r}
library(TxDb.Hsapiens.UCSC.hg38.knownGene)
# updated 29 Sep 2016
hg <- TxDb.Hsapiens.UCSC.hg38.knownGene
seqlevels(hg) <- c("chrX")
ABCD1 <-genes(hg,  filter = list(gene_id = "215"))

#ranges(ABCD1)
# clean up 
rm(hg)
# Create two sliding windows
mywindows <- slidingWindows(ABCD1, width = 19894, step = 1 )
mywindows
```

`@sct`
```{r}
msg1 <- "Incorrect. The length of the genome will be much longer than the length of our gene of interest "
msg2 <- "Correct! The length of the gene -1 will allow us to have at least 2 windows. The length of the last window can be partial"
msg3 <- "Incorrect. If you use exactly the same width of the gene, then you will only get one window"
msg4 <- "Incorrect. If you choose a width lower than the length of the GRanges object we will have many more windows"
ex() %>% check_mc(2, feedback_msgs = c(msg1, msg2, msg3, msg4))
```

---

## Is it there?

```yaml
type: MultipleChoiceExercise
key: 95b5169650
lang: r
xp: 50
skills: 1
```

`GRangesLists` also come with useful accessor functions; almost all the accessors 
from `IRanges` and `GRanges` are reused, but the result will be a list instead.
You can find accessors using the function `methods(class = "GRangesList")`.

It is your turn to explore chromosome X genes `hg_chrX`, and find the gene of interest `ABCD1`.
Using the function `overlapsAny()` between the target `ABCD1` and the subject `hg_chrX`. 

Are there any overlapping ranges with the `ABCD1` gene?

You can test it using `overlapsAny(ABCD1, hg_chrX)`, then `table`, which will help you count the values.

`@possible_answers`
- There are no overlapping ranges
- There is no gene `ABCD1`
- [Yes, there is an overlap between hg_chrX and the gene ABCD1!]

`@hint`
Use `overlapsAny(ABCD1, hg_chrX)` to see whether this is `TRUE` or `FALSE`.
Then, enclose `table` to the resulting list.

`@pre_exercise_code`
```{r}
library(TxDb.Hsapiens.UCSC.hg38.knownGene)
# updated March 17
hg <- TxDb.Hsapiens.UCSC.hg38.knownGene
seqlevels(hg) <- c("chrX")

hg_chrX <- genes(hg, filter = list(tx_chrom = "chrX"))

ABCD1 <- genes(hg,  filter = list(gene_id = "215"))

rm(hg)
```

`@sct`
```{r}
msg1 <- "Incorrect. Have you tried the `overlapsAny()` function?"
msg2 <- "Incorrect. Check the object `ABCD1` to make sure it exists."
msg3 <- "Correct! Yes! our gene of interest overlaps somewhere in chromosome X. In the next exercise you will find out where."
ex() %>% check_mc(3, feedback_msgs = c(msg1, msg2, msg3))
```

---

## How many transcripts?

```yaml
type: NormalExercise
key: 9a172f544b
lang: r
xp: 100
skills: 1
```

Remember in the video how we discovered the length of the exons in one of the transcripts of our gene of interest?
It is your turn to find out how many transcripts does `ABCD1` has. You can find out by using

```
transcriptsBy(x, by = "gene")
```

A little tip is to use tick marks like this `215` with the gene id when selecting from the transcripts object.

`@instructions`
- load the human transcripts DB to hg
- prefilter chromosome X using `seqlevels()`
- get all transcripts by gene and store it in `hg_chrXt`
- select gene `215` from the transcripts using `$`

`@hint`
A little tip is to enclose `215` the gene id when selecting from the transcripts object.
like this: 
``` 
hg_chrXt$`215` 
```

`@pre_exercise_code`
```{r}
library(TxDb.Hsapiens.UCSC.hg38.knownGene)
```

`@sample_code`
```{r}
# load the human transcripts DB to hg
library(TxDb.Hsapiens.UCSC.hg38.knownGene)
hg <- ___

# prefilter chromosome X
___

# get all transcripts by gene
hg_chrXt <- transcriptsBy(____)

# select gene `215` from the transcripts
___
```

`@solution`
```{r}
# load the human transcripts DB to hg
library(TxDb.Hsapiens.UCSC.hg38.knownGene)
hg <- TxDb.Hsapiens.UCSC.hg38.knownGene

# prefilter chromosome X
seqlevels(hg) <- c("chrX")

# get all transcripts by gene
hg_chrXt <- transcriptsBy(hg, by = "gene")

# select gene `215` from the transcripts
hg_chrXt$`215`
```

`@sct`
```{r}
ex() %>% check_object("hg")
ex() %>% check_code("TxDb.Hsapiens.UCSC.hg38.knownGene", times=2)
ex() %>% check_expr("seqlevels(hg)") %>% check_result() %>% check_equal()
ex() %>% check_correct(
	check_object(.,"hg_chrXt") %>% check_equal(),
  	check_function(.,"transcriptsBy") %>% {
      check_arg(.,"x") %>% check_equal(eval=FALSE)
      check_arg(.,"by") %>% check_equal(eval=FALSE)
    }
)
ex() %>% check_output_expr("hg_chrXt$`215`", missing_msg="Have you correctly selected gene 215 from the transcripts?")


success_msg("I’m amazed by your ability to link concepts you just learned! Keep up the effort you will learn more, and I hope it is fun too!")
```

---

## More about ABCD1

```yaml
type: NormalExercise
key: e4479f9c6e
lang: r
xp: 100
skills: 1
```

Now that you know that there is an overlap between chromosome X and the gene ABCD1, let's find its gene_id and its location, also called [locus](https://www.ncbi.nlm.nih.gov/books/NBK5191/#IX-L).

`@instructions`
- Use the function `subsetByOverlaps` to subset `hg_chrX` with its overlapping region with the `ABCD1` gene, and store the overlapping range in `rangefound`
- check `names` of `rangefound`
- check the `ABCD1` 
- check `rangefound`

`@hint`
- First create an object called `rangefound` with the results of the subset with the two key objects the chromosome and the gene of interest
- Second look for the names on this range
- Third you can print the gene of interest and the range found

`@pre_exercise_code`
```{r}
library(TxDb.Hsapiens.UCSC.hg38.knownGene)
# updated March 17
hg <- TxDb.Hsapiens.UCSC.hg38.knownGene
seqlevels(hg) <- c("chrX")
hg_chrX <- genes(hg, filter = list(tx_chrom = "chrX"))
ABCD1 <-genes(hg,  filter = list(gene_id = "215"))
rm(hg)
```

`@sample_code`
```{r}
# Store the overlapping range in rangefound
___ <- ___(hg_chrX, ABCD1)

# Check names of rangefound
___(___)

# Check the geneOfInterest 
___

# Check rangefound
___
```

`@solution`
```{r}
# Store the overlapping range in rangefound
rangefound <- subsetByOverlaps(hg_chrX, ABCD1)

# Check names of rangefound
names(rangefound)

# Check the geneOfInterest 
ABCD1

# Check rangefound
rangefound
```

`@sct`
```{r}
ex() %>% check_correct(
	check_object(.,"rangefound") %>% check_equal(),
   	check_function(.,"subsetByOverlaps") %>% {
      check_arg(.,"x") %>% check_equal()
      check_arg(.,"ranges") %>% check_equal()
    }
)
ex() %>% check_function("names") %>% check_arg("x") %>% check_equal()
ex() %>% check_output_expr("ABCD1")
ex() %>% check_output_expr("rangefound")



success_msg("Great work! you did pretty well figuring out the name of the gene_id of `ABCD1` is 215 which is an ENTREZ id, and its exact location is chrX:153724868-153744762:+ !")
```

---

## From GRangesList object into a GRanges object

```yaml
type: MultipleChoiceExercise
key: 6037171647
lang: r
xp: 50
skills: 1
```

The `unlist` operation is fast, and serves to partition a `GRangesList`. 

You can unlist the `hg_chrX` and then check how the lengths differ between the `GRangesList` and the `GRanges` objects. You can test in the console:

- `unlist` `hg_chrX` and save a new object called `myGR` 
- Check the `class()` of both objects
- Check the `lengths()` of both objects

Would you expect the `GRanges` object to have a length ____ the `GRangesList`?

`@possible_answers`
- [greater than]
- lower than
- equal to

`@hint`
Remember that each element in the list has one or more ranges

`@pre_exercise_code`
```{r}
library(TxDb.Hsapiens.UCSC.hg38.knownGene)
# updated March 17
hg <- TxDb.Hsapiens.UCSC.hg38.knownGene
seqlevels(hg) <- c("chrX")
hg_ChrX <- transcriptsBy(hg, by = "gene")
class(hgChrX)
length(hgChrX)
```

`@sct`
```{r}
msg1 <- "Correct! Yes! because the `GRanges` has now all of the ranges in the elements of the list."
msg2 <- "Incorrect. A `GRanges` object derived from a `GRangesList` can only have the same or larger length."
msg3 <- "Incorrect. In this case, the `GRanges` object derived from the GRangesList has a larger length."
ex() %>% check_mc(1, feedback_msgs = c(msg1, msg2, msg3))
```
