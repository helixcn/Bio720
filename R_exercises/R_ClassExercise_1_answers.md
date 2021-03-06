---
title: "Bio720 Introduction to `R`, in class exercise"
author: "Ian Dworkin"
date: "October 3, 2016"
output: html_document
---
## Overview
In class tonight we are going to both practice some of the `R` skills you were introduced to in the video tutorials. [Click here](https://github.com/DworkinLab/Bio720/blob/master/Introduction_to_R.md) for that list.

The learning objectives for today are as follows:

1. Learn some best practices for organizing computational projects (i.e. any projects with some scripts, data and outputs).
2. Learn some intuitive (but not necessarily technical) ideas about *data structures* in general, and review some of the data structures in `R`.
3. Practice some of the skills that were introduced in the video tutorials.

## How to organize computational projects.

Please [click here](https://github.com/DworkinLab/Bio720/blob/master/IntroductionMarkdownAndVersionControl/Bio720_IntroductionMarkdown.md#a-few-words-on-project-organization) to link to a brief discussion on these points.

## Some very basic thoughts on *data structures* in `R`
We are not going to have a computer science-esque discussion of data structures (there are whole courses on this), but instead try to introduce a few basic concepts to understand why computers need to store different types of data in different ways, and why we need to be aware of that.

### What is the point of data structures? (class discussion)
- What kind of data do we want the computer to store for us?
- Why does it matter what kind of data (integers, floating point numbers, strings, Boolean,...)?

### Data structures in R

As was discussed in the video screencasts R has a number of different basic data structures (and more can be made or extended by programmers like you!). We need to start with the so-called *atomic* types that can be stored as vectors (remember R does not have an object to store scalars!). You can google them, but they are logical (Boolean), integer, real (double or floating point) , complex, string (or character) and raw. Let's think about a few of them:




```r
x <- 1
```

and you can find out information about this with a variety of functions:


```r
str(x)
```

```
##  num 1
```

```r
mode(x)
```

```
## [1] "numeric"
```

```r
typeof(x)
```

```
## [1] "double"
```
Why does `mode(x)` and `typeof(x)` give different results?


Let's create a few more objects and take a look at them


```r
y = c(3, 4, 5)
```

Will `x` and `y` differ?  Check and explain?

Now let's create a new object z:


```r
z = 3:5
```
How should `y` and `z` compare? how would you check?


```r
mode(y)
```

```
## [1] "numeric"
```

```r
mode(z)
```

```
## [1] "numeric"
```

```r
typeof(y)
```

```
## [1] "double"
```

```r
typeof(z)
```

```
## [1] "integer"
```
 Which might suggest they are different. In one case `R` is treating the vector as integers, the other case as floating point (double). So this suggests they might be different. However:
 

```r
y == z
```

```
## [1] TRUE TRUE TRUE
```

```r
all.equal(y, z)
```

```
## [1] TRUE
```

```r
# BUT
identical(y, z)
```

```
## [1] FALSE
```

Suggests R is treating them the same. This is definitely one of the odd R behaviours. While in many languages (like `C++`) you need to define the type of variable you are creating, R tries to make "guesses" about what you are doing. Sometimes this can result in odd behaviour.

### R does its calculations as `double`, even if inputs are integers
even though `z` is a vector with integer values, if you use z in some calculations that require double precision floating point values, it will be converted to double. In other words::


```r
typeof(z)
```

```
## [1] "integer"
```

```r
# because of how division is computed in R, it converts to double
typeof(z/z)
```

```
## [1] "double"
```

```r
#  though, addition, substraction and multiplication do not require this, so stay integer
typeof(z*z)
```

```
## [1] "integer"
```
Most of the time this is not an issue, but it is a strange thing.


### Some other atomic types in R
Ok, let's think about some of the other basic data types we learned about (strings or "character" in R, boolean/logical)


```r
rm( x, y, z) # clean up
x = 1
y = "1"
z = "one"
a = TRUE
b = "TRUE"
```

Before checking, think about what types each of these objects should be. Then check.

```r
x == y
```

```
## [1] TRUE
```

```r
all.equal(x, y)
```

```
## [1] "Modes: numeric, character"              
## [2] "target is numeric, current is character"
```

```r
identical(x, y)
```

```
## [1] FALSE
```

How about `y` and `z`?


```r
y == z
```

```
## [1] FALSE
```

```r
all.equal(y, z)
```

```
## [1] "1 string mismatch"
```

```r
identical(y, z)
```

```
## [1] FALSE
```

```r
# Which might be obvious by
mode(y)
```

```
## [1] "character"
```

```r
mode(z)
```

```
## [1] "character"
```



```r
y == z
```

```
## [1] FALSE
```

```r
all.equal(y, z)
```

```
## [1] "1 string mismatch"
```

```r
identical(y, z)
```

```
## [1] FALSE
```

```r
# Which might be obvious by
mode(y)
```

```
## [1] "character"
```

```r
mode(z)
```

```
## [1] "character"
```

The same "logic" applies for comparing `a` and `b`


```r
a == b
```

```
## [1] TRUE
```

```r
all.equal(a, b)
```

```
## [1] "Modes: logical, character"              
## [2] "target is logical, current is character"
```

```r
identical(a, b)
```

```
## [1] FALSE
```

```r
# Which might be obvious
mode(a); typeof(a)
```

```
## [1] "logical"
```

```
## [1] "logical"
```

```r
mode(b); typeof(b)
```

```
## [1] "character"
```

```
## [1] "character"
```

So, what are the take home points?
- Computers can be very exact.
- R has some confusing ideas about storing variables, assigning types to basic variables and you need to think carefully about what "equals" means.

Group exercises: How would you get `R` to coerce `x` and `y` to be exactly the same? How about `a` and `b`? 

Let's coerce b into a logical/Boolean

```r
b1 <- as.logical(b)
is.logical(b1); is.character(b1)
```

```
## [1] TRUE
```

```
## [1] FALSE
```

```r
b == b1
```

```
## [1] TRUE
```

```r
a == b
```

```
## [1] TRUE
```

```r
typeof(b1); mode(b1)
```

```
## [1] "logical"
```

```
## [1] "logical"
```

Let's coerce a into a character

```r
a1 <- as.character(a)
is.character(a1)
```

```
## [1] TRUE
```

```r
a == a1
```

```
## [1] TRUE
```

```r
a == b
```

```
## [1] TRUE
```

```r
typeof(a1); mode(a1)
```

```
## [1] "character"
```

```
## [1] "character"
```

And, y into a number

```r
y1 <- as.numeric(y)
y == y1
```

```
## [1] TRUE
```

```r
identical(y, y1)
```

```
## [1] FALSE
```

```r
x == y1
```

```
## [1] TRUE
```

```r
identical(x, y1)
```

```
## [1] TRUE
```

```r
typeof(y1); mode(y1)
```

```
## [1] "double"
```

```
## [1] "numeric"
```

### Boolean/logical
Using TRUE/FALSE (logical, Boolean) as an atomic type is quite important. Often we need to check equality of objects, or elements within our objects (considering vectors are a basic storage type in R). These are particularly (as we will see later) useful when subsetting using the index of a vector (or matrix or data.table).

Let's clean up a bit:


```r
rm(x, y, z, a, b)
```

As we will see, both T or TRUE can be used. Likewise for F or FALSE. It is also important to know you don't want these to be treated as strings/characters so don't put quotes around them.


```r
x = T
x
```

```
## [1] TRUE
```

```r
y = TRUE
y
```

```
## [1] TRUE
```

```r
x == y
```

```
## [1] TRUE
```

```r
identical(x, y)
```

```
## [1] TRUE
```

It is also useful to know that TRUE has a numeric value associated with it (1), and FALSE is associated with 0. 


```r
sum(x)
```

```
## [1] 1
```

```r
as.numeric(x)
```

```
## [1] 1
```

```r
a = F
sum(a)
```

```
## [1] 0
```

Before running the code to find out, what will the sum of the following vector be `c(rep(T, 10), rep(F, 18), rep(T, 10), rep(F, 6))`


```r
sum(c(rep(T, 10), rep(F, 18), rep(T, 10), rep(F, 6)))
```

```
## [1] 20
```

Take home message: Boolean values of TRUE and FALSE have numeric values of 1 and 0 respectively. This can be very useful for subsetting by columns or rows for matrices and data.frames!

## Building up our data structures. 

Now that we have some better idea (hopefully) of some of the atomic data types, we want to use these to build more complex data structures that may (eventually - like next week) be useful for data analysis, simulations and the like. There are a few important ones that we will use a lot: matrix, list, data.frame, factors, and formula (which we will not cover in Bio720 but is essential for statistical analyses). There are other important ones (like array) but we will cover these other ones first.

Before we get any further and create some new objects, how do we see all of the objects we currently have in our global environment?


```r
ls()
```

```
## [1] "a"  "a1" "b1" "x"  "y"  "y1"
```

Let's work with a clean slate. How might we remove all of the objects and start fresh? Obviously you could just do a `rm()` command with each object name, but you can also remove all at once.


```r
rm(list=ls())
ls()
```

```
## character(0)
```

Let's think about what this command has done.


Now we are going to create a few new objects and use these to examine some of the properties of our more complex data structures


```r
gene1 <- c(3, 4, 7, 9, 12, 6)
gene2 <- c(11, 17, 12, 25, 23, 7)
gene3 <- c(100, 103, 97, 94, 106, 111)
```
What mode and type should these objects be?


## understanding `factors` in R.

Create an object `genotype` of length 6, where the first three observations have the value "wildtype" and the last three are "mutant"

There are at least three options. First the hard way.

```r
genotype <- c("wildtype", "wildtype", "wildtype", "mutant", "mutant", "mutant")
genotype
```

```
## [1] "wildtype" "wildtype" "wildtype" "mutant"   "mutant"   "mutant"
```

```r
mode(genotype)
```

```
## [1] "character"
```

A pretty easy way

```r
genotype2 <- rep(c("wildtype", "mutant"), each = 3)
genotype2
```

```
## [1] "wildtype" "wildtype" "wildtype" "mutant"   "mutant"   "mutant"
```

```r
mode(genotype2)
```

```
## [1] "character"
```

```r
class(genotype2)
```

```
## [1] "character"
```

Or if we want to generate factors immediately we can use the `gl()` function (for *generate levels*):


```r
genotype3 <- gl(n = 2, k = 3, labels = c("wildtype", "mutant"))
genotype3
```

```
## [1] wildtype wildtype wildtype mutant   mutant   mutant  
## Levels: wildtype mutant
```

```r
mode(genotype3)
```

```
## [1] "numeric"
```

```r
class(genotype3)
```

```
## [1] "factor"
```

Now this last approach is pretty interesting for a couple of reasons. First the `class` of the object is factor not character. Second the mode of the object is numeric. What is going on here?


First compare these different objects, genotype (or genotype2 which is identical) and genotype3 (using gl). Are they the same?


```r
genotype2 == genotype3
```

```
## [1] TRUE TRUE TRUE TRUE TRUE TRUE
```

```r
identical(genotype2, genotype3)
```

```
## [1] FALSE
```

```r
all.equal(genotype2, genotype3)
```

```
## [1] "Modes: character, numeric"                      
## [2] "Attributes: < target is NULL, current is list >"
## [3] "target is character, current is factor"
```

So let's think about what a factor is?

factors in R may appear as `character` but for efficiency are stored as integers. The idea is you will have far fewer factor levels (which you can check with `nlevels()`) than number of observations, so this can save memory and speed up computation. However, this means you need to realize that factors are not a special form of `character`, but a special form of `numeric`!

If we wanted to make genotype2 into a factor (we will call it genotype2_factor) how would you do so?


```r
genotype2_factor <- as.factor(genotype2)
class(genotype2_factor)
```

```
## [1] "factor"
```

```r
mode(genotype2_factor)
```

```
## [1] "numeric"
```

```r
identical(genotype3, genotype2_factor)
```

```
## [1] FALSE
```

```r
genotype3 == genotype2_factor
```

```
## [1] TRUE TRUE TRUE TRUE TRUE TRUE
```
This is frankly an unfortunate behaviour of R's. Most other things were done on purpose. This probably was not!


How about if we wanted to make genotype3 into a character vector?


```r
genotype3_character <- as.character(genotype3)
genotype3_character 
```

```
## [1] "wildtype" "wildtype" "wildtype" "mutant"   "mutant"   "mutant"
```

```r
class(genotype3_character)
```

```
## [1] "character"
```

```r
mode(genotype3_character)
```

```
## [1] "character"
```

```r
identical(genotype3_character, genotype2)
```

```
## [1] TRUE
```

```r
genotype3_character == genotype2
```

```
## [1] TRUE TRUE TRUE TRUE TRUE TRUE
```

Let's say we had a second experimental factor which was the day of sampling (3,6) but we want to treat it as a factor `c(3, 6, 3, 6, 3, 6)` how would you code this?


```r
day <- gl(n = 2, k = 1 , length = 6, labels = c(3, 6))
day
```

```
## [1] 3 6 3 6 3 6
## Levels: 3 6
```

```r
class(day)
```

```
## [1] "factor"
```

```r
mode(day)
```

```
## [1] "numeric"
```

```r
typeof(day)
```

```
## [1] "integer"
```

What happens if you coerce day into a character?


```r
as.character(day)
```

```
## [1] "3" "6" "3" "6" "3" "6"
```

How about if you coerce day into numeric?

```r
as.numeric(day)
```

```
## [1] 1 2 1 2 1 2
```

Seemingly strange behaviour? However think about it for a minute and try to explain it.

The basic idea is that when a variable is stored as a `factor` in R, the first level (which defaults to alphanumeric, so **m**utant before **w**ildtype in this case) will be stored as "1", the second level as "2" and so on. When you ask to convert it to numeric it uses these numbers. So if your factor levels are named with numbers to begin with, this can mess things up. So take care!

So if you want to turn these into the numbers 3 and 6, how would you do it?

```r
as.numeric(as.character(day))
```

```
## [1] 3 6 3 6 3 6
```

Take home message: factors are useful for storing names of experimental levels efficiently, but keep in mind they are stored internally as numbers, not strings! 

## Back to our data structures of interest. 

Provide two different ways of combining `gene1`, `gene2` and `gene3` into a matrix (gene_mat1 and gene_mat2)?


```r
gene_mat1 <- cbind(gene1, gene2, gene3)
gene_mat1
```

```
##      gene1 gene2 gene3
## [1,]     3    11   100
## [2,]     4    17   103
## [3,]     7    12    97
## [4,]     9    25    94
## [5,]    12    23   106
## [6,]     6     7   111
```

```r
gene_mat2 <- matrix(c(gene1, gene2, gene3), nrow =6, ncol =3, byrow =FALSE)
```
Are these the same?


```r
gene_mat1 == gene_mat2
```

```
##      gene1 gene2 gene3
## [1,]  TRUE  TRUE  TRUE
## [2,]  TRUE  TRUE  TRUE
## [3,]  TRUE  TRUE  TRUE
## [4,]  TRUE  TRUE  TRUE
## [5,]  TRUE  TRUE  TRUE
## [6,]  TRUE  TRUE  TRUE
```

```r
identical(gene_mat1, gene_mat2)
```

```
## [1] FALSE
```

Using some of the tools we have already used (str, mode, typeof) shows the elements are the same. However, one has stripped the names (gene_mat2), why do you think this has happened?

How might you fix this?

It is pretty easy, since it is just names that differ, you can use `colnames` to rename the columns

```r
colnames(gene_mat2) <- c("gene1", "gene2", "gene3")
gene_mat2
```

```
##      gene1 gene2 gene3
## [1,]     3    11   100
## [2,]     4    17   103
## [3,]     7    12    97
## [4,]     9    25    94
## [5,]    12    23   106
## [6,]     6     7   111
```

```r
identical(gene_mat1, gene_mat2)
```

```
## [1] TRUE
```

Let's take our (character) vectors for day and genotype and use `cbind()` (treatments). Before starting write down whether you think the object `treatments` will have class `matrix`. What will the mode be? Why?

```r
genotype <- rep(c("wildtype", "mutant"), each = 3)
day <- rep(c("3", "6"), times = 3)

genotype
```

```
## [1] "wildtype" "wildtype" "wildtype" "mutant"   "mutant"   "mutant"
```

```r
day
```

```
## [1] "3" "6" "3" "6" "3" "6"
```

```r
treatments <- cbind(genotype, day)
class(treatments)
```

```
## [1] "matrix"
```

```r
mode(treatments)
```

```
## [1] "character"
```

Now let's take all of  objects that are vectors of different atomic types (gene1, gene2, gene3, genotype, day) and use cbind on them. Call this object `all_the_data`. Before writing the code, write down what you think the class of the object will be. How about the mode/type of the elements of `all_the_data`?


```r
all_the_data <- cbind(gene1, gene2, gene3, genotype, day)
class(all_the_data)
```

```
## [1] "matrix"
```

```r
mode(all_the_data)
```

```
## [1] "character"
```

Explain why `all_the_data` is the class and has the mode that it does?
Again R is trying to be smart. It can not coerce words into numbers, but it can coerce numbers into strings/characters. So, to keep this a matrix, it first coerces everything into character, and then makes a matrix out of them. Take a look at `?matrix` and it gives some information on the coercion hierarchy. It is worth having this in the back of your mind. 

## data structures with heterogeneous objects.
 Clearly we did not want to produce a matrix of strings. So we need some sort of data structures where elements (at least at the level of individual vectors that are being organized together) can be of different atomic types (i.e. a collection of heterogeneous objects). There are two main approaches to this, one is the data.frame, which is the spreadsheet like object that is often the easiest to work with when importing data (for analysis and plotting). THe other is a list. As I mentioned in the video tutorials, the data.frame is really a special kind of list. However it is worth comparing and contrasting both. First remove the old `all_the_data` object and make a new one that is a data frame.

## `data.frames`
First let's make a data.frame: 


```r
rm(all_the_data)
all_the_data <- data.frame(gene1, gene2, gene3, genotype, day)
str(all_the_data)
```

```
## 'data.frame':	6 obs. of  5 variables:
##  $ gene1   : num  3 4 7 9 12 6
##  $ gene2   : num  11 17 12 25 23 7
##  $ gene3   : num  100 103 97 94 106 111
##  $ genotype: Factor w/ 2 levels "mutant","wildtype": 2 2 2 1 1 1
##  $ day     : Factor w/ 2 levels "3","6": 1 2 1 2 1 2
```

```r
class(all_the_data)
```

```
## [1] "data.frame"
```

```r
mode(all_the_data)
```

```
## [1] "list"
```

Notice a couple of interesting thing. First it's class is a data.frame, but it is actually a list underneath. Second, without asking or warning us, it has coerced *genotype* and *day* into factors. It is assuming that since you are treating this like regular data (that you will probably want to analyze or plot) you want these as factors. Often this is true. If you don't want this behaviour there is an argument that you can set `stringsAsFactors == FALSE`.

It is really important to note that data.frames are useful for heterogeneous objects **ONLY IF** all objects (vectors) are the **same length**. It is ok to have missing data, as long as R knows there should be missing data (NA) in certain spots. When you need to store a collection of heterogeneous objects, but the objects are of different lengths, then you need to use lists.

As we showed in the video tutorial, you can extract and subset in a couple of ways (like lists or as a matrix). So show three different ways to extract the 2nd, 3rd and 4th column from `all_the_data` 

First way, using standard numeric subsetting:


```r
all_the_data[ ,c(2:4)]
```

```
##   gene2 gene3 genotype
## 1    11   100 wildtype
## 2    17   103 wildtype
## 3    12    97 wildtype
## 4    25    94   mutant
## 5    23   106   mutant
## 6     7   111   mutant
```


second, by subsetting using the names of the columns:


```r
all_the_data[c("gene2", "gene3", "genotype")]
```

```
##   gene2 gene3 genotype
## 1    11   100 wildtype
## 2    17   103 wildtype
## 3    12    97 wildtype
## 4    25    94   mutant
## 5    23   106   mutant
## 6     7   111   mutant
```

Third, by using the extraction operator `$` which is how you extract elements from lists:


```r
all_the_data$gene2; all_the_data$gene3; all_the_data$genotype
```

```
## [1] 11 17 12 25 23  7
```

```
## [1] 100 103  97  94 106 111
```

```
## [1] wildtype wildtype wildtype mutant   mutant   mutant  
## Levels: mutant wildtype
```
This approach is useful for single columns, not so useful when you want to extract a bunch though.

You can also use the `[[` approach to grab columns as well

```r
all_the_data[["gene2"]]
```

```
## [1] 11 17 12 25 23  7
```

```r
all_the_data$gene2
```

```
## [1] 11 17 12 25 23  7
```

We can also use the subset() function, which is far more powerful and we will use alot in the coming weeks. 


```r
subset(all_the_data, select = c("gene2", "gene3", "genotype"))
```

```
##   gene2 gene3 genotype
## 1    11   100 wildtype
## 2    17   103 wildtype
## 3    12    97 wildtype
## 4    25    94   mutant
## 5    23   106   mutant
## 6     7   111   mutant
```

Take home message: This can definitely get confusing, but different programmers still use each, so it is important to recognize what is the same and what is different. Importantly the single `[` can be used to extract more than one element from the object, while `$` and `[[` can only select a single element at a time. 


Hopefully it is pretty clear, but there are a couple of ways of adding on new variables. Let's create a gene4 (also of length 6) and add it to the `all_the_data` data.frame


```r
all_the_data$gene4 <- c(10, 11, 7, 11, 2, 3)

all_the_data
```

```
##   gene1 gene2 gene3 genotype day gene4
## 1     3    11   100 wildtype   3    10
## 2     4    17   103 wildtype   6    11
## 3     7    12    97 wildtype   3     7
## 4     9    25    94   mutant   6    11
## 5    12    23   106   mutant   3     2
## 6     6     7   111   mutant   6     3
```

```r
str(all_the_data)
```

```
## 'data.frame':	6 obs. of  6 variables:
##  $ gene1   : num  3 4 7 9 12 6
##  $ gene2   : num  11 17 12 25 23 7
##  $ gene3   : num  100 103 97 94 106 111
##  $ genotype: Factor w/ 2 levels "mutant","wildtype": 2 2 2 1 1 1
##  $ day     : Factor w/ 2 levels "3","6": 1 2 1 2 1 2
##  $ gene4   : num  10 11 7 11 2 3
```

### Lists in `R`

When you need to store a collection of heterogeneous objects, but the objects are of different lengths, then you need to use lists. As you saw above with the `$` and the `[[` operators, you can extract things from lists. However, making lists is simpler than unmaking (well unlisting) lists as we will see.

Make a list called `list_the_data` using the same objects that were used to make `all_the_data`. What will the class of the object be? how about the mode of the objects within the list?


```r
list_the_data = list(gene1, gene2, gene3, genotype, day)
list_the_data
```

```
## [[1]]
## [1]  3  4  7  9 12  6
## 
## [[2]]
## [1] 11 17 12 25 23  7
## 
## [[3]]
## [1] 100 103  97  94 106 111
## 
## [[4]]
## [1] "wildtype" "wildtype" "wildtype" "mutant"   "mutant"   "mutant"  
## 
## [[5]]
## [1] "3" "6" "3" "6" "3" "6"
```

```r
str(list_the_data)
```

```
## List of 5
##  $ : num [1:6] 3 4 7 9 12 6
##  $ : num [1:6] 11 17 12 25 23 7
##  $ : num [1:6] 100 103 97 94 106 111
##  $ : chr [1:6] "wildtype" "wildtype" "wildtype" "mutant" ...
##  $ : chr [1:6] "3" "6" "3" "6" ...
```

```r
names(list_the_data)
```

```
## NULL
```
A couple of things of note:
- It should be pretty clear that there is something different about the way it is storing the information. Seemingly the names of the original objects have been lost. 
- Also, as a list, it does not make assumptions about how you will use the underlying objects, so it has not coerced the character vectors to factors. 

How might we get the names of the underlying objects? Annoyingly:



```r
list_the_data = list(gene1 = gene1, gene2 = gene2, gene3 = gene3, genotype = genotype, day = day)
list_the_data
```

```
## $gene1
## [1]  3  4  7  9 12  6
## 
## $gene2
## [1] 11 17 12 25 23  7
## 
## $gene3
## [1] 100 103  97  94 106 111
## 
## $genotype
## [1] "wildtype" "wildtype" "wildtype" "mutant"   "mutant"   "mutant"  
## 
## $day
## [1] "3" "6" "3" "6" "3" "6"
```

```r
str(list_the_data)
```

```
## List of 5
##  $ gene1   : num [1:6] 3 4 7 9 12 6
##  $ gene2   : num [1:6] 11 17 12 25 23 7
##  $ gene3   : num [1:6] 100 103 97 94 106 111
##  $ genotype: chr [1:6] "wildtype" "wildtype" "wildtype" "mutant" ...
##  $ day     : chr [1:6] "3" "6" "3" "6" ...
```

```r
names(list_the_data)
```

```
## [1] "gene1"    "gene2"    "gene3"    "genotype" "day"
```

We can also have objects that have different lengths within the list


```r
list_the_data$random_variable = c(T,T,F) 

list_the_data
```

```
## $gene1
## [1]  3  4  7  9 12  6
## 
## $gene2
## [1] 11 17 12 25 23  7
## 
## $gene3
## [1] 100 103  97  94 106 111
## 
## $genotype
## [1] "wildtype" "wildtype" "wildtype" "mutant"   "mutant"   "mutant"  
## 
## $day
## [1] "3" "6" "3" "6" "3" "6"
## 
## $random_variable
## [1]  TRUE  TRUE FALSE
```

```r
str(list_the_data)
```

```
## List of 6
##  $ gene1          : num [1:6] 3 4 7 9 12 6
##  $ gene2          : num [1:6] 11 17 12 25 23 7
##  $ gene3          : num [1:6] 100 103 97 94 106 111
##  $ genotype       : chr [1:6] "wildtype" "wildtype" "wildtype" "mutant" ...
##  $ day            : chr [1:6] "3" "6" "3" "6" ...
##  $ random_variable: logi [1:3] TRUE TRUE FALSE
```
 
We can extract variables from lists in a slight variant of the approach we have used so far

 

```r
list_the_data$gene1
```

```
## [1]  3  4  7  9 12  6
```

```r
list_the_data[1]
```

```
## $gene1
## [1]  3  4  7  9 12  6
```

```r
list_the_data["gene1"]
```

```
## $gene1
## [1]  3  4  7  9 12  6
```

```r
list_the_data[[1]]
```

```
## [1]  3  4  7  9 12  6
```

```r
list_the_data[["gene1"]]
```

```
## [1]  3  4  7  9 12  6
```

However, these objects are not all equivalent


```r
class(list_the_data$gene1)
```

```
## [1] "numeric"
```

```r
class(list_the_data[1])
```

```
## [1] "list"
```

```r
class(list_the_data["gene1"])
```

```
## [1] "list"
```

```r
class(list_the_data[[1]])
```

```
## [1] "numeric"
```

```r
class(list_the_data[["gene1"]])
```

```
## [1] "numeric"
```

```r
str(list_the_data$gene1)
```

```
##  num [1:6] 3 4 7 9 12 6
```

```r
str(list_the_data[1])
```

```
## List of 1
##  $ gene1: num [1:6] 3 4 7 9 12 6
```

```r
str(list_the_data["gene1"])
```

```
## List of 1
##  $ gene1: num [1:6] 3 4 7 9 12 6
```

```r
str(list_the_data[[1]])
```

```
##  num [1:6] 3 4 7 9 12 6
```

```r
str(list_the_data[["gene1"]])
```

```
##  num [1:6] 3 4 7 9 12 6
```

So using the `[` operator keeps the information (and variable name) as a list, while the `$` or `[[` operators extract just the elements, but do not keep the name. It can not always be coerced in a sensible way as we have done before.

i.e:


```r
as.numeric(list_the_data[1])
```

```
## Error in eval(expr, envir, enclos): (list) object cannot be coerced to type 'double'
```

You can `unlist` the vector though

```r
str(as.numeric(unlist(list_the_data[1])))
```

```
##  num [1:6] 3 4 7 9 12 6
```

However, this also strips off the name! So you are best to not use the `[` if you can avoid it when using lists. This is not always possible though, so knowing about unlist is useful!


