---
title: 'What is Bioconductor?'
description: 'In this chapter you will get hands-on with Bioconductor. Bioconductor is the specialized  repository for bioinformatics software, developed and maintained by the R community. You will learn how to install and use bioconductor packages. You will be introduced to S4 objects and functions, because most packages within Bioconductor inherit from S4. Additionally, you will use a real genomic dataset of a fungus to explore the BSgenome package.'
free_preview: true
attachments:
    slides_link: 'https://s3.amazonaws.com/assets.datacamp.com/production/course_6307/slides/chapter1.pdf'
---

## Introduction to the Bioconductor Project

```yaml
type: VideoExercise
key: bebaf2d02f
lang: r
xp: 50
skills: 1
```

`@projector_key`
819c82e82d856db4e334ce5d9ffd69c9

---

## Bioconductor version

```yaml
type: NormalExercise
key: def4276bb4
lang: r
xp: 100
skills: 1
```

In the video, you learned about the Bioconductor project. One advantage of this fantastic resource is its continuous improvements, reflected in scheduled releases. Hence, checking the current version is important for the reproducibility of your analysis.  

The package `BiocInstaller` is already installed for you, using `source("https://bioconductor.org/biocLite.R")`

Remember, you can check the Bioconductor version using the function `biocVersion()` from `BiocInstaller`.

`@instructions`
Your task is to check the Bioconductor version currently installed
- First, check the version using the syntax `package::function()`
- Then, check the version using only the name of the `function()`

`@hint`
Use the `BiocInstaller` package with the function
`biocVersion()`

`@pre_exercise_code`
```{r}
source("https://bioconductor.org/biocLite.R")
# should be 3.6
```

`@sample_code`
```{r}
# Load the BiocInstaller package
library(BiocInstaller)

# Explicit syntax to check the Bioconductor version
___

# When BiocInstaller is loaded use biocVersion alone
___
```

`@solution`
```{r}
# Load the BiocInstaller package
library(BiocInstaller)

# Explicit syntax to check the Bioconductor version
BiocInstaller::biocVersion()

# When BiocInstaller is loaded use biocVersion alone
biocVersion()
```

`@sct`
```{r}
ex() %>% check_code("BiocInstaller::biocVersion()")
ex() %>% check_function("biocVersion", index=1)
ex() %>% check_function("biocVersion", index=2)



success_msg("Well done! this will help you in your documentation when citing the Bioconductor version you are using")
```

---

## BiocLite to install packages

```yaml
type: NormalExercise
key: 056c840c9f
lang: r
xp: 100
skills: 1
```

The `BSgenome` package has been installed for you. This is a data package which contains representations of several genomes. 

    source("https://bioconductor.org/biocLite.R")
    biocLite("BSgenome")


Any bioconductor package can be installed using the code above, then loaded using `library(packageName)`. To check the package's version, use the function `packageVersion("packageName")`.

`@instructions`
- Load the package `BSgenome`. 
- Read the message and find required packages (no need to add code here)
- Check the installed version of `BSgenome`

`@hint`
- Use `library()` to load `BSgenome`
- Use `packageVersion("packageName")` to check the version

`@pre_exercise_code`
```{r}
# no pre code
```

`@sample_code`
```{r}
# Load the BSgenome package
___

# Check the version of the BSgenome package
___
```

`@solution`
```{r}
# Load the BSgenome package
library(BSgenome)

# Check the version of the BSgenome package
packageVersion("BSgenome")
```

`@sct`
```{r}
ex() %>% check_library("BSgenome")
ex() %>% check_function("packageVersion") %>% check_arg("pkg") %>% check_equal(eval=FALSE)



success_msg("Excellent! using the code from this exercise you can load and check the version of any package from Bioconductor")
```

---

## The role of S4 in Bioconductor

```yaml
type: VideoExercise
key: 454596463f
lang: r
xp: 50
skills: 1
```

`@projector_key`
214f687a3a3ce5f6e01ab65b27e4c626

---

## S4 class definition

```yaml
type: MultipleChoiceExercise
key: a656e98cb8
lang: r
xp: 50
skills: 1
```

We will use the class `BSgenome`, which is already loaded for you.

Let's check the formal definition of this class by using the function
`showClass("className")`
- Check the `BSgenome` class results and find which are the parent classes (**Extends**) and the classes that inherit from it (**Subclasses**).

`@possible_answers`
- No classes
- BSgenome
- GenomeDescription
- [GenomeDescription and MaskedBSgenome]

`@hint`
Use `showClass("BSgenome")` to check the formal definition of this class. Check **Extends** and **Subclasses** at the end of the results.

`@pre_exercise_code`
```{r}
library(BSgenome)
```

`@sct`
```{r}
msg1 <- "Incorrect. There are at least 1 class that extends from BSgenome and 1 class that is a subclass of BSgenome"
msg2 <- "Incorrect. BSgenome does not inherit from itself"
msg3 <- "Incorrect. But close to the correct answer. GenomeDescription is the class parent of BSgenome"
msg4 <- "Correct! Both ways of inheritance. BSgenome extends from GenomeDescription and has MaskedBSgenome as subclass"
ex() %>% check_mc(4, feedback_msgs = c(msg1, msg2, msg3, msg4))
success_msg("Well done! The `BSgenome` is a powerful class and inherits from `GenomeDescription` which you will see later on, and has `MaskedBSgenome` as subclass")
```

---

## Interaction with classes

```yaml
type: NormalExercise
key: 050febf3ce
lang: r
xp: 100
skills: 1
```

Let's say we have an object called `a_genome`, from class `BSgenome`, you can ask questions like these: 

    # Check its main class
    class(a_genome)  # "BSgenome"
    
    # Check its other classes
    is(a_genome)  # "BSgenome", "GenomeDescription"
    
    # Is it an S4 representation?
    isS4(a_genome)  # TRUE
    

If you want to find out more about it, you can use the accessor `show(a_genome)` or have a look for information using other specific accessors, from the list of `.S4methods(class = "BSgenome")`.

`@instructions`
- Investigate about the `a_genome` using `show()`
- Investigate some other accesors like `organism()`, `provider()`, `seqinfo()`, one per line

`@hint`
- Use the `show()` function with `a_genome` for a nice summary of a genome object.
- Use `organism()`, `provider()`, `seqinfo()` with `a_genome`, one per line to get specific information about the genome object.

`@pre_exercise_code`
```{r}
#load a genome
library(BSgenome.Scerevisiae.UCSC.sacCer3)
a_genome <- BSgenome.Scerevisiae.UCSC.sacCer3
```

`@sample_code`
```{r}
# Investigate about the a_genome using show()
___

# Investigate some other accesors
organism(____)
provider(____)
seqinfo(____)
```

`@solution`
```{r}
# Investigate about the a_genome using show()
show(a_genome)

# Investigate some other accesors 
organism(a_genome)
provider(a_genome)
seqinfo(a_genome)
```

`@sct`
```{r}
ex() %>% check_function("show") %>% check_arg("object") %>% check_equal(eval=FALSE)
ex() %>% check_function("organism") %>% check_arg("object") %>% check_equal(eval=FALSE)
ex() %>% check_function("provider") %>% check_arg("x") %>% check_equal(eval=FALSE)
ex() %>% check_function("seqinfo") %>% check_arg("x") %>% check_equal(eval=FALSE)



success_msg("Keep up the good work, you can now check other objects and investigate if they are S4 objects, their classes and their accessors. Remember you can use `.S4methods()` or `showMethods()` to check the accessors list of a class or a function")
```

---

## Introducing biology of genomic datasets

```yaml
type: VideoExercise
key: ad48090635
lang: r
xp: 50
skills: 1
```

`@projector_key`
be55dda28c78f0a9339cb65915b89a90

---

## Discovering the Yeast genome

```yaml
type: NormalExercise
key: ff8659b293
lang: r
xp: 100
skills: 1
```

Let's continue to explore the yeast genome, which is already installed for you. In this exercise, you will load it and assign it:

```{r}
library(BSgenome.Scerevisiae.UCSC.sacCer3)
yeastGenome <- BSgenome.Scerevisiae.UCSC.sacCer3
```

As with other data in R, we can use `head()` and `tail()` to explore the `yeastGenome` object. We can also look subset the genome by chromosome by using the `$`, in this way `object_name$chromosome_name`. Ask `names()` if in doubt.

Another nifty function is `nchar()`, used to count the number of characters in a sequence.

`@instructions`
Using the `yeastGenome`:
- `head()` the sequence names `seqnames()`
- `tail()` the sequence lengths `seqlengths()`
- select `$` chromosome M `chrM`
- count `nchar()` from `chrM`

`@hint`
- Make sure you use the `yeastGenome` for each point listed in the instructions.
- You have to run each point separately.
- The first two points use a function inside another function

`@pre_exercise_code`
```{r}
library(BSgenome.Scerevisiae.UCSC.sacCer3)
yeastGenome <- BSgenome.Scerevisiae.UCSC.sacCer3
```

`@sample_code`
```{r}
# Load the yeast genome
library(BSgenome.Scerevisiae.UCSC.sacCer3)

# Assign data to the yeastGenome object
yeastGenome <- ____

# Get the head of seqnames and tail of seqlengths for yeastGenome
head(seqnames(____))
tail(seqlengths(___))

# Select chromosome M, alias chrM
___

# Count characters of the chrM sequence
___
```

`@solution`
```{r}
# Load the yeast genome
library(BSgenome.Scerevisiae.UCSC.sacCer3)

# Assign data to the yeastGenome object
yeastGenome <- BSgenome.Scerevisiae.UCSC.sacCer3

# Get the head of seqnames and tail of seqlengths for yeastGenome
head(seqnames(yeastGenome))
tail(seqlengths(yeastGenome))

# Select chromosome M, alias chrM
yeastGenome$chrM

# Count characters of the chrM sequence
nchar(yeastGenome$chrM)
```

`@sct`
```{r}
ex() %>% check_object("yeastGenome") %>% check_equal()
ex() %>% check_correct(
	check_function(.,"head") %>% check_arg("x") %>% check_equal(),
	check_function(.,"seqnames") %>% check_arg("x") %>% check_equal()
)
ex() %>% check_correct(
	check_function(.,"tail") %>% check_arg("x") %>% check_equal(),
	check_function(.,"seqlengths") %>% check_arg("x") %>% check_equal()
)
ex() %>% check_output_expr("yeastGenome$chrM", missing_msg="Did you select `chrM` from `yeastGenome`?") 
ex() %>% check_function("nchar") %>% check_arg("x") %>% check_equal()



success_msg("Excellent job! You have practiced sequence subsetting and next comes another subsetting exercise!")
```

---

## Partitioning the Yeast genome

```yaml
type: NormalExercise
key: edeb30fb45
lang: r
xp: 100
skills: 1
```

Genomes are often big, but interest lies in specific regions, so we need to subset a genome by extracting parts of it. To do this, use `getSeq()`. You can also use this function with some optional extra info:

    getSeq(yeastGenome, names = "chrI", start = 100, end = 150)

`@instructions`
Using the object `yeastGenome` 
- Use `getSeq()` to get the first 30 bases of each chromosome

`@hint`
Employ the `getSeq()` function from the `BSgenome` package for more information
`?getSeq`.

`@pre_exercise_code`
```{r}
# load genome
library(BSgenome.Scerevisiae.UCSC.sacCer3)
yeastGenome <- BSgenome.Scerevisiae.UCSC.sacCer3
```

`@sample_code`
```{r}
# Load the yeast genome
library(BSgenome.Scerevisiae.UCSC.sacCer3)

# Assign data to the yeastGenome object
yeastGenome <- BSgenome.Scerevisiae.UCSC.sacCer3

# Get the first 30 bases of each chromosome
___
```

`@solution`
```{r}
# Load the yeast genome
library(BSgenome.Scerevisiae.UCSC.sacCer3)

# Assign data to the yeastGenome object
yeastGenome <- BSgenome.Scerevisiae.UCSC.sacCer3

# Get the first 30 bases of each chromosome of the a_genome object
getSeq(yeastGenome, end = 30)
```

`@sct`
```{r}
ex() %>% check_correct(
  	check_output_expr(.,"getSeq(yeastGenome, end = 30)"),
  	check_function(.,"getSeq") %>% {
      check_arg(.,"x") %>% check_equal(eval=FALSE)
      check_arg(.,"end") %>% check_equal()
    }
)

success_msg("Congratulations, this might have been the beginning of your first sequence analysis!")
```

---

## Available Genomes

```yaml
type: MultipleChoiceExercise
key: b9b6304252
lang: r
xp: 50
skills: 1
```

As a recap, the `BSgenome` package makes available various public genomes. If you want to explore the available genomes from this package, you can use

    available.genomes()
    
The list of names will appear in the following format: *BSgenome.speciesName.provider.version*. After running this function, can you tell which is the major provider of available genomes?

`@possible_answers`
- 1000genomes
- JGI
- NCBI
- UCSC

`@hint`
The list of names will be in the following format: BSgenome.speciesName.provider.version.
You can also check `?available.genomes`

`@pre_exercise_code`
```{r}
library(BSgenome)
```

`@sct`
```{r}
msg1 <- "Incorrect. The 1000 Genome Project has made available only the Homo sapiens genome `BSgenome.Hsapiens.1000genomes.hs37d5`"
msg2 <- "Incorrect. The Joint Genome Institute or JGI, has made available only the plant genome of Arabidopsis lyrata `BSgenome.Alyrata.JGI.v1`"
msg3 <- "Incorrect. The National Center for Biotechnology Information NCBI has made available three genomes, but there is another provider of more genomes. Keep trying."
msg4 <- "Correct! The University of California, Santa Cruz (UCSC) Genome Browser has made available the most genomes for `BSgenome`, totaling 74 of various species."
ex() %>% check_mc(4, feedback_msgs = c(msg1, msg2, msg3, msg4))
```
