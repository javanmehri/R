#Merging Data

Sometimes You will load in more than one dataset into R and you will want to be able to merge the datasets together. Usually what you will want to do is match those datasets based on an ID.

This is very similar to the idea of having a linked set of tables and a database like My SQL.

Here we download two datasets from web to start working on them. Data are collected on people who submitted answers to questions. And then people who reviewed those answers and tried to decide whether they were right or not. This is a model system for how the peer review system works in scientific studies.

```r
> setwd(“./bin/G-C-Data")
> if(!file.exists("./data")){dir.create("./data")}
> fileUrl1 = "https://dl.dropboxusercontent.com/u/7710864/data/reviews-apr29.csv"
> fileUrl2 = "https://dl.dropboxusercontent.com/u/7710864/data/solutions-apr29.csv"
> download.file(fileUrl1,destfile="./data/reviews.csv",method="curl")
…
> download.file(fileUrl2,destfile="./data/solutions.csv",method=“curl")
…

```
```r
> reviews = read.csv("./data/reviews.csv"); solutions <- read.csv("./data/solutions.csv")

> head(reviews,2)
  id solution_id reviewer_id      start       stop time_left accept
1  1           3          27 1304095698 1304095758      1754      1
2  2           4          22 1304095188 1304095206      2306      1

> head(solutions,2)
  id problem_id subject_id      start       stop time_left answer
1  1        156         29 1304095119 1304095169      2343      B
2  2        269         25 1304095119 1304095183      2329      C
```
As you can see one of the variables in “reviews” is “solution_id”, and that corresponds to the id that appears in the “solutions” dataset.
```r
> names(reviews)
[1] "id"          "solution_id" "reviewer_id" "start"       "stop"       
[6] "time_left"   "accept"     

> names(solutions)
[1] "id"         "problem_id" "subject_id" "start"      "stop"      
[6] "time_left"  "answer"       
```
## Important parameters

x,y	are two data frames

You can use either “by” or “by.x” or “by.y” to tell a merge which columns it should merge by.

All	by default it merges by all of the columns that have the common name.

## Merge() Function
```r
> mergedData = merge(reviews, solutions, by.x="solution_id", by.y="id", all=TRUE)
```
all=TRUE means if there is a value that appears in one but in the other, it should include another row with na values for the missing values that are nor appeared in the other data frame.
```r
> head(mergedData)
  solution_id id reviewer_id    start.x     stop.x time_left.x accept problem_id
1           1  4          26 1304095267 1304095423        2089      1        156
2           2  6          29 1304095471 1304095513        1999      1        269
3           3  1          27 1304095698 1304095758        1754      1         34
4           4  2          22 1304095188 1304095206        2306      1         19
5           5  3          28 1304095276 1304095320        2192      1        605
6           6 16          22 1304095303 1304095471        2041      1        384
  subject_id    start.y     stop.y time_left.y answer
1         29 1304095119 1304095169        2343      B
2         25 1304095119 1304095183        2329      C
3         22 1304095127 1304095146        2366      C
4         23 1304095127 1304095150        2362      D
5         26 1304095127 1304095167        2345      A
6         27 1304095131 1304095270        2242      C
```
For example you can check the information related to solution_id of 2 in both reviews and solutions datasets and mergedData: 
```r
> head(reviews)
  id solution_id reviewer_id      start       stop time_left accept
1  1           3          27 1304095698 1304095758      1754      1
2  2           4          22 1304095188 1304095206      2306      1
3  3           5          28 1304095276 1304095320      2192      1
4  4           1          26 1304095267 1304095423      2089      1
5  5          10          29 1304095456 1304095469      2043      1
6  6           2          29 1304095471 1304095513      1999      1

> head(solutions)
  id problem_id subject_id      start       stop time_left answer
1  1        156         29 1304095119 1304095169      2343      B
2  2        269         25 1304095119 1304095183      2329      C
3  3         34         22 1304095127 1304095146      2366      C
4  4         19         23 1304095127 1304095150      2362      D
5  5        605         26 1304095127 1304095167      2345      A
6  6        384         27 1304095131 1304095270      2242      C
```

```r
> names(reviews)
[1] "id"          "solution_id" "reviewer_id" "start"       "stop"        "time_left"   "accept"     
> names(solutions)
[1] "id"         "problem_id" "subject_id" "start"      "stop"       "time_left"  "answer"    
> intersect(names(solutions), names(reviews))
[1] "id"        "start"     "stop"      "time_left"
```

If you try to merge without telling to merge on the basis of, it will try to merge on the basis of all those four variables.

```r
> mergedData2 = merge(reviews, solutions, all=TRUE)
> head(mergedData2)
  id      start       stop time_left solution_id reviewer_id accept problem_id subject_id answer
1  1 1304095119 1304095169      2343          NA          NA     NA        156         29      B
2  1 1304095698 1304095758      1754           3          27      1         NA         NA   <NA>
3  2 1304095119 1304095183      2329          NA          NA     NA        269         25      C
4  2 1304095188 1304095206      2306           4          22      1         NA         NA   <NA>
5  3 1304095127 1304095146      2366          NA          NA     NA         34         22      C
6  3 1304095276 1304095320      2192           5          28      1         NA         NA   <NA>
```

## Using join in the plyr package

It is faster but it only can merge on the basis of common rows, common names, between the two datasets.

Here isvtwo made up data frames with a common id identifier, but I've sampled them, so they're in different orders.

```r
> df1= data.frame(id=sample(1:10), x=rnorm(10))
> df1
   id          x
1   7 -1.0449338
2   9 -0.2934225
3   3 -1.2616827
4  10 -0.3067413
5   8 -0.2654366
6   1  1.6190957
7   4 -0.7091066
8   5  1.7436235
9   2  1.8176360
10  6  1.5566880

> df2= data.frame(id=sample(1:10), y=rnorm(10))
> df2
   id          y
1   1  0.6465424
2   5 -0.3564381
3  10  0.2316846
4   3  1.7315007
5   7  1.7511216
6   9  1.6989233
7   6  0.3934196
8   4 -0.5598206
9   8  1.9083346
10  2  1.3776001
```
```r
> library(plyr)

> join(df1, df2)
Joining by: id
   id          x          y
1   7 -1.0449338  1.7511216
2   9 -0.2934225  1.6989233
3   3 -1.2616827  1.7315007
4  10 -0.3067413  0.2316846
5   8 -0.2654366  1.9083346
6   1  1.6190957  0.6465424
7   4 -0.7091066 -0.5598206
8   5  1.7436235 -0.3564381
9   2  1.8176360  1.3776001
10  6  1.5566880  0.3934196
```

The arrange command arranges them in increasing order by id.

```r
> arrange(join(df1, df2), id)
Joining by: id
   id          x          y
1   1  1.6190957  0.6465424
2   2  1.8176360  1.3776001
3   3 -1.2616827  1.7315007
4   4 -0.7091066 -0.5598206
5   5  1.7436235 -0.3564381
6   6  1.5566880  0.3934196
7   7 -1.0449338  1.7511216
8   8 -0.2654366  1.9083346
9   9 -0.2934225  1.6989233
10 10 -0.3067413  0.2316846
```

The nice thing about the plyer package, and the reason why I bring it up is that if you have multiple data frames it’s relatively challenging to do it with the merge command. But if they have a common id, it's very straightforward to do it with the join all command.

```r
> df1= data.frame(id=sample(1:10), x=rnorm(10))
> df2= data.frame(id=sample(1:10), y=rnorm(10))
> df3= data.frame(id=sample(1:10), z=rnorm(10))
> dfList = list(df1,df2,df3)
> join_all(dfList)
Joining by: id
Joining by: id
   id          x          y           z
1   7 -1.0449338  1.7511216 -0.34969297
2   9 -0.2934225  1.6989233  0.21036144
3   3 -1.2616827  1.7315007 -0.18043834
4  10 -0.3067413  0.2316846 -0.84299152
5   8 -0.2654366  1.9083346 -1.78384967
6   1  1.6190957  0.6465424 -0.78424402
7   4 -0.7091066 -0.5598206 -0.16377282
8   5  1.7436235 -0.3564381 -0.21771277
9   2  1.8176360  1.3776001  0.09788326
10  6  1.5566880  0.3934196  0.26504490
```

Credit: Jeff Leek, Assistant Professor of Biostatistics at the Johns Hopkins Bloomberg School of Public Health

