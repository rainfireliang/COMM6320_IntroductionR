---
title: "R: Review"
author: "Hai Liang - hailiang@cuhk.edu.hk"
output: pdf_document
---
# Introduction
This is a brief introduction to R for both programming and data analysis. 

R is not the only tool for computational studies. Sometimes, it is not the best one either. We choose R for instruction just because we, social scientists, find it's easy to learn. It is not a statistical package with programing functions.

When we are talking about computational methods, two techniques are involved: programming and data analysis. Programming is different from data analysis. They are two different things, though highly correlated in computational studies. Many students can't tell the differences at the very beginning of learning. They read papers from computer science, statistics, and social sciences. The topics are similar but may have fundamental differences in techniques.

Programmers can develop fantastic applications, such as MS word and PPT. They possibly know nothing about data analysis and didn't take a statistics course.

On the other side, data analyst may have little knowledge on coding. I guess most of you may have some basic knowledge about statistics, but can't write a statistical program using C, JAVA, or Python. You do statistical analysis by simply clicking the buttons in SPSS or STATA. If SPSS doesn't provide a function you needed, you have no idea.

In this sense, Python is a good tool for programming, while R is good for data analysis. Data analysis doesn't merely refer to statistics. Machine learning and other data mining models could not be statistical (that means we don't infer parameters with distribution assumptions). 

In section I, this tutorial will focus on programming. In section II, this tutorial will talk about data analysis.

The first step to use R is to set the working directory. Your working directory is the folder on your computer
in which you are currently working. When you ask R to open a file, it will look in the working directory for this file, and when you tell R to save a data file or figure, it will save it in the working directory.

```{r}
setwd("D:/Dropbox/Teaching Materials/CUHK Courses/Digital Research/Slides/Labs/R Review") # set a working directory
```
R can do many statistical and data analyses. They are organized in so-called packages or libraries.
With the standard installation, most common packages are installed.

```{r}
#install.packages("tm")
#library("tm") # load a library (remember library names)
#help(package="tm")
```
# Section I: Programming Basics
## 1.1. I/O
How to import data from text, csv files into R?
```{r}
#data = read.table(file="authorlist.txt",stringsAsFactors=F)
#data = read.csv(file="authorlist.csv",stringsAsFactors=F)
#write.table(data,file="authorlist.txt")
#write.csv(data,file="authorlist.csv")
```
It's also possible to import data from SPSS or STATA files, even you haven't installed the software. We use the foreign library.

```{r}
library(foreign)
#read.spss(file="authorlist.sav", to.data.frame = T)
#read.dta(file="authorlist.dta")
#save(data,file="")
#load("sample.Rdata")
```

## 1.2. Basic syntax
Let's do some basic calculations. 
```{r}
1+2
1-2
1*2
1/2
sqrt(2) # squared root of 2
2^2 # 2 squared
```

We can define a variable and assign a value to it.

```{r}
height = 180 # instead of using the equation symbol, it is equivalent to use the arrow symbol.
weight = 50
print (height*weight)
```
or alternatively, we assign the result to a new variable
```{r}
nv = height*weight
nv # instead of print(nv)
```

R has some generic functions we can use them directly. For example,calculating the sum and mean of a series of numbers.If you don't how to use a function, you can use the help function.

```{r}
sum(c(1,2,3))
mean(c(1,2,3))
```
If you don't know how to use a function, you can use the help() function and check the examples. For example
```{r}
help(sum) # or equivalently
?sum
```

## 1.3. Data types
There are 3 general modes of data in R: string/characters, numbers, and logical.

```{r}
string = "I am xxx"
string

# use class or typeof to determine the type of a variable
class(string)
typeof(string)

number = 5
number

```
Another important data type is the logical type. There are two predefined variables:TRUE and FALSE
```{r}
logic = TRUE
logic
missing = NA
missing
```

## 1.4. Data structures
To learn any programming languages, a shortcut is to learn the core elements. Fortunately, most programming languages share very similar core elements. They are data structure, conditional expressions, loops, and functions. I will illustrate these one by one

Four types of data structure are commonly used in R: vector, matrix, data frame, and list. Here is a good illustration.

The first is vector. Vector is a list of values. These values could be numeric, logic, or string.
```{r}
# using c (c is a function) to generate a new vector
v = c(1,7,9,10) 
v
```
The data type of vector values must be the same. If input numbers,characters,and missing value together, what will happen? 
```{r}
v2 = c(1,7,NA,"hi")
v2
# to access an element in the vector
v[-2]
# vector elements can have names
names(v)
# assign names
names(v) = c("First","Second","Third","Fourth")
names(v)
```
We can apply mathematical calculations directly to vector.We can multiply a vector by a number, calculate the sqrt of each elements in the vector.

```{r}
v*2
sqrt(v)
```
The second structure is matrix. Matrix is a collection of vectors, organized in rows and columns (nothing more than 2-dimensional vectors). Values in a matrix should be in the same data type. Each column or row is a vector. We use the matrix function to generate a matrix with 5 cols and 5 rows. Values are the integers from 1 to 25.

```{r}
m = matrix(1:25,5,5) 
```
Similar to vector, we could do simple maths.
```{r}
m+2
m+m
m%*%m # matrix nultiplication
```
We can combine two matrices by cols or rows.
```{r}
cbind(m,m) 
rbind(m,m)
# access a matrix element
m[2,3] = "hi"
```
A data frame is a matrix, while columns could be of all different data types. 

```{r}
v1=c(1,7,6,3,2)
v2=c(T,F,T,F,T)
v3=c("Y","N","Y","N","Y")
d = data.frame(v1,v2,v3,stringsAsFactors=F) # create use data.frame
```
A list is a collection of all data structure.
```{r}
l = list(v,m,d)
names(l)
l[[3]]
```

## 1.5. Programming tools
I will introduce 3 programming tools: if...else expression (which is one of the conditional expressions); the for and while loop expressions, and how to define your own functions.

Here is a task: input a number, if the value is larger than or equal to 100, print "High Value", if the value is less than 100 and larger than or equal to 50, print "Middle Value", if less than 50, print "Low Value".

```{r}
x = 95 
if (x>=100){
  print("High Value")
} else if (x>=50&x<100){
  print("Middle Value")
} else {
  print("Low Value")
}
```
Task: calculate the number of characters in our names. We use nchar() for each name.That means we will use the nchar() function many times.

```{r}
names=c("Hai","Jonanthan","Zhenzhen","Qinjie")
for (name in names) {
  N = nchar(name)
  print (paste(name,":",N,"characters"))
}
```
or alternatively, loop over the index of the vector:
```{r}
for (i in 1:4) {
  N = nchar(name[i])
  print (paste(name[i],":",N,"characters"))
}
```
or use while instead of loop

```{r}
i=1
while (i<=4){
  N = nchar(name[i])
  print (paste(name[i],":",N,"characters"))
  i=i+1 #counter
}
```
You can write your own functions and use them in the same way as the generic functions. For example, you can write a function to calculate the area of a square. The formula is area = width*height.

```{r}
area = function(height=5,width=20){
  area = height*width
  return (area)
}
area(11,10)
```
# Section II: Data Management and Analysis
Section II will focus on data management and analysis. Our data are usually stored in data frames in R. Sometimes, we need to transform the original data into different format in order to use existing R functions for data analysis. Data management is the procedure to prepare our data for formal analyses.

## 2.1. Data management
I will use the attached sample data throughout the section. First, load the Rdata file.

```{r}
load("sample.Rdata") # actually the data frame call tst3 (not sample)
```
There are 6 variables and 1000 observations. This data was download from New York Times (I will tell you how to collect this data tomorrow). id: the id of the article, section_name: the section of the article, publication date, abstract, the lead paragraph, and word count. The type of the original word count is character. Now, we can create a new variable wc, which is the numeric type of word count.

```{r}
tst3$wc = as.numeric(tst3$word_count)
```
We can recode the wc variable into a binary variable "long" using the within function. If wc>500, the long variable is "Yes". That means this is a long article.

```{r}
tst3 = within(tst3,{
  long = NA # initiate a new variable
  long[wc>500] = "Yes"  
  long[wc <= 500] = "No" })
```
We can rename the colnames or the variable names.

```{r}
colnames(tst3)[2] = "section"
```
Deal with missing value is also straightforward.
```{r}
tst3$wc[501] = NA # let the value to be missing
#is.na(tst3$wc) # using is.na() to check the missing value
tst3$wc[is.na(tst3$wc)]=0 # replace the missing value with 0
tst3$wc[501]
```
We should pay particular attention the NA value. Sometime, it causes big problems.
```{r}
sum(c(1,2,3,NA))
sum(c(1,2,3,NA),na.rm=T)
```
We use the square bracket to extract part of the data frame.
```{r}
# to select the first and third column
newd_1 = tst3[,c(1,3)] 
# the same
newd_2 = tst3[,c("id","pub_date")]
# delete the second and third col
newd_3 = tst3[,c(-2,-3)]
# select cols with name "section_name" and "pub_date"
newd_4 = tst3[!(names(tst3)%in%c("section_name","pub_date"))]
# del the id col
newd_4$id = NULL
# select cases (keep the long articles)
newd_5 = tst3[which(tst3$long=="Yes"),]
```
Aggregating is useful to summarize data. For example, calculate the average length by section. We want to get a new data frame with two variables. Col 1 is the section and col 2 is the average length of the articles. We use the aggregate() function.

```{r}
agg_1 = aggregate(wc~section,tst3,FUN="mean")
agg_2 = aggregate(tst3$wc,by=list(tst3$section),FUN="mean") # agg_1 & agg_2 are exactly the same.
# calculate the number of artiles in each section
tst3$n=1
agg_3 = aggregate(cbind(wc,n)~section,tst3,FUN="sum")
```
In agg1, we have two variables: section and avg. Now, we want to merge agg1 and tst3 into 1 single data frame to see whether the word count of an article is larger than its section average. In the new data frame, append the avg variable into tst3. We can use merge() function.

```{r}
colnames(agg_1)[2] = "avg"
tst3 = merge(tst3,agg_1,by="section")
head(tst3)
```
Reshaping is a very powerful tool for data management. The example I show here is similar to the excercise 2.
Check the tst3 data frame. We find that some articles belong to more than 1 sections. Suppose we want to know how many times any two sections co-occurent. e.g., Arts and Books co-occurent 5 times.

```{r}
# split the section variable by semicolon. "Arts;Books" will be split into two words: Arts and Books.
sections = sapply(tst3$section,function(x)strsplit(x,split="; "))
# we extract the first word
A = sapply(sections,function(x)x[[1]])
# we extract the second word if any (here we assume at most two)
B = sapply(sections,function(x) ifelse(length(x)>1,x[[2]],NA))
# combine A and B to a new data frame
longd = data.frame(A,B,stringsAsFactors=F)
longd = longd[!is.na(longd$B),] # remove na
head(longd)
```
Now we use the reshape package to reshape the current format to a co-occurence matrix.
```{r}
library(reshape)
longd$freq<-1
withagg<-cast(longd,A~B,sum)
withagg[1:10,1:10]
rev<-melt(withagg,id=c("A"))
head(rev)
```

## 2.2. Descriptive statistics
Now, I am going to introduce some basic statistical analysis using R. You should notice that most statistical functions are only applicable for data frames. The most simple one is to use summary() to show the sumary statistics.
```{r}
summary(tst3)
```
Another way is to use by and summary together to summary different groups. For example, summary statistics for the long and short articles separately.
```{r}
by(tst3[,c("wc","avg")],tst3$long,summary)

# create a cross table
table(tst3$section,tst3$long)

# conduct chi-square test
#install.packages("vcd")
library(vcd)
mytable<-xtabs(~section+long, data=tst3)
chisq.test(mytable) # chisq test

assocstats(mytable) # categorical association

cor(tst3$wc,tst3$avg,method="spearman",use="complete.obs")
cor.test(tst3$wc,tst3$avg, alternative = "two.side", method ="pearson" )

t.test(wc~long,tst3)

fit<-aov(wc~long,data=tst3)
summary(fit)
```

## 2.3. Graphs
Just a few simple examples, if you want to know more about R graphing, please read the manual of ggplot packages.

```{r}
count<-table(tst3$long)
barplot(count,main="Simple Bar Plot",
        xlab="Long article or not", ylab="Frequency")
barplot(count,main="Simple Bar Plot",
        xlab="Long article or not", ylab="Frequency", horiz=TRUE)

hist(tst3$wc,breaks=20, 
     col="red",
     xlab="World count",
     main="Colored histogram with 20 bins")
hist(tst3$wc,breaks=20, 
     freq = F,
     col="red",
     xlab="World count",
     main="Colored histogram with 20 bins")
rug(jitter(tst3$wc))
lines(density(tst3$wc), col="blue", lwd=2)

plot(tst3$wc,tst3$avg,
     main="Basic Scatter plot of wc  vs.avg", xlab="Word count",
     ylab="Section average", pch=19)
abline(lm(tst3$wc~tst3$avg), col="red", lwd=2,   lty=1)
lines(lowess(tst3$wc,tst3$avg), col="blue", lwd=2, lty=2)
```
