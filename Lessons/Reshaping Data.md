# Reshaping Data

## Start with reshaping

```r
> install.packages(“reshape2”)
…
> library(reshape2)

```
Now we want to reshape a dataset a little bit.

```r
> head(mtcars)
                   mpg cyl disp  hp drat    wt  qsec vs am gear carb
Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4
Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4
Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1
Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2
Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1

```

##  Melting Data frames

```r
> rownames(mtcars)
 [1] "Mazda RX4"           "Mazda RX4 Wag"      
 [3] "Datsun 710"          "Hornet 4 Drive"     
 [5] "Hornet Sportabout"   "Valiant"            
 [7] "Duster 360"          "Merc 240D"          
 [9] "Merc 230"            "Merc 280"           
[11] "Merc 280C"           "Merc 450SE"         
[13] "Merc 450SL"          "Merc 450SLC"        
[15] "Cadillac Fleetwood"  "Lincoln Continental"
[17] "Chrysler Imperial"   "Fiat 128"           
[19] "Honda Civic"         "Toyota Corolla"     
[21] "Toyota Corona"       "Dodge Challenger"   
[23] "AMC Javelin"         "Camaro Z28"         
[25] "Pontiac Firebird"    "Fiat X1-9"          
[27] "Porsche 914-2"       "Lotus Europa"       
[29] "Ford Pantera L"      "Ferrari Dino"       
[31] "Maserati Bora"       "Volvo 142E"      

> mtcars$carname <- rownames(mtcars)
> head(mtcars)
                   mpg cyl disp  hp drat    wt  qsec vs am gear carb          carnames
Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4         Mazda RX4
Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4     Mazda RX4 Wag
Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1        Datsun 710
Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1    Hornet 4 Drive
Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2 Hornet Sportabout
Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1           Valiant

```
The following command makes “carname","gear" and “cyl" column as id and melts “mpg" and “hp" columns into one. So data will now a tall skinny data set.

```r
> carMelt <- melt(mtcars, id=c("carname","gear", "cyl"), measure.vars=c("mpg","hp"))

> carMelt
               carname gear cyl variable value
1            Mazda RX4    4   6      mpg  21.0
2        Mazda RX4 Wag    4   6      mpg  21.0
3           Datsun 710    4   4      mpg  22.8
4       Hornet 4 Drive    3   6      mpg  21.4
5    Hornet Sportabout    3   8      mpg  18.7
6              Valiant    3   6      mpg  18.1
7           Duster 360    3   8      mpg  14.3
8            Merc 240D    4   4      mpg  24.4
9             Merc 230    4   4      mpg  22.8
10            Merc 280    4   6      mpg  19.2
11           Merc 280C    4   6      mpg  17.8
12          Merc 450SE    3   8      mpg  16.4
13          Merc 450SL    3   8      mpg  17.3
14         Merc 450SLC    3   8      mpg  15.2
15  Cadillac Fleetwood    3   8      mpg  10.4
16 Lincoln Continental    3   8      mpg  10.4
17   Chrysler Imperial    3   8      mpg  14.7
18            Fiat 128    4   4      mpg  32.4
19         Honda Civic    4   4      mpg  30.4
20      Toyota Corolla    4   4      mpg  33.9
21       Toyota Corona    3   4      mpg  21.5
22    Dodge Challenger    3   8      mpg  15.5
23         AMC Javelin    3   8      mpg  15.2
24          Camaro Z28    3   8      mpg  13.3
25    Pontiac Firebird    3   8      mpg  19.2
26           Fiat X1-9    4   4      mpg  27.3
27       Porsche 914-2    5   4      mpg  26.0
28        Lotus Europa    5   4      mpg  30.4
29      Ford Pantera L    5   8      mpg  15.8
30        Ferrari Dino    5   6      mpg  19.7
31       Maserati Bora    5   8      mpg  15.0
32          Volvo 142E    4   4      mpg  21.4
33           Mazda RX4    4   6       hp 110.0
34       Mazda RX4 Wag    4   6       hp 110.0
35          Datsun 710    4   4       hp  93.0
36      Hornet 4 Drive    3   6       hp 110.0
37   Hornet Sportabout    3   8       hp 175.0
38             Valiant    3   6       hp 105.0
39          Duster 360    3   8       hp 245.0
40           Merc 240D    4   4       hp  62.0
41            Merc 230    4   4       hp  95.0
42            Merc 280    4   6       hp 123.0
43           Merc 280C    4   6       hp 123.0
44          Merc 450SE    3   8       hp 180.0
45          Merc 450SL    3   8       hp 180.0
46         Merc 450SLC    3   8       hp 180.0
47  Cadillac Fleetwood    3   8       hp 205.0
48 Lincoln Continental    3   8       hp 215.0
49   Chrysler Imperial    3   8       hp 230.0
50            Fiat 128    4   4       hp  66.0
51         Honda Civic    4   4       hp  52.0
52      Toyota Corolla    4   4       hp  65.0
53       Toyota Corona    3   4       hp  97.0
54    Dodge Challenger    3   8       hp 150.0
55         AMC Javelin    3   8       hp 150.0
56          Camaro Z28    3   8       hp 245.0
57    Pontiac Firebird    3   8       hp 175.0
58           Fiat X1-9    4   4       hp  66.0
59       Porsche 914-2    5   4       hp  91.0
60        Lotus Europa    5   4       hp 113.0
61      Ford Pantera L    5   8       hp 264.0
62        Ferrari Dino    5   6       hp 175.0
63       Maserati Bora    5   8       hp 335.0
64          Volvo 142E    4   4       hp 109.0

```

## Casting Data Frames

```r
> cylData <- dcast(carMelt, cyl~variable)
> cylData

  cyl mpg hp
1   4  11 11
2   6   7  7
3   8  14 14

```

This shows the cyl variables breaked down to variable (cyl on rows and variable on columns).

For example for 4 cylinder cars we have 11 measures of mpg and 11 measures of hp. 


```r
> cylData <- dcast(carMelt, cyl~variable, mean)
> cylData

  cyl      mpg        hp
1   4 26.66364  82.63636
2   6 19.74286 122.28571
3   8 15.10000 209.21429
```
This time it shows the mean of the variable column of data set which contains mpg and hp for each cylinder.

## Averaging Values

```r
> head(InsectSprays)
  count spray
1    10     A
2     7     A
3    20     A
4    14     A
5    14     A
6    12     A
```
We want the sum of the count for eachv of the different sprays.

This will apply sum function to count along the index spray:

```r
> tapply(InsectSprays$count, InsectSprays$spray, sum)
  A   B   C   D   E   F
174 184  25  59  42 200
```

## Another way - split

```r
> spIns = split(InsectSprays$count, InsectSprays$spray)

```
It takes the count variables and splits them up by each of the different sprays. So you will have a list of values for “A”, “B” and so forth.

```r
> spIns
$A
 [1] 10  7 20 14 14 12 10 23 17 20 14 13

$B
 [1] 11 17 21 11 16 14 17 17 19 21  7 13

$C
 [1] 0 1 7 2 3 1 2 1 3 0 1 4

$D
 [1]  3  5 12  6  4  3  5  5  5  5  2  4

$E
 [1] 3 5 3 5 3 6 1 1 3 2 6 4

$F
 [1] 11  9 15 22 15 16 13 10 26 26 24 13
```

## Another way - lapply

```r
> sprCount = lapply(spIns, sum)
```
Apply sum function for each element of the spIns

```r
> sprCount
$A
[1] 174

$B
[1] 184

$C
[1] 25

$D
[1] 59

$E
[1] 42

$F
[1] 200
```

## Another way - combine

```r
> unlist(sprCount)
  A   B   C   D   E   F
174 184  25  59  42 200
```
sapply will do both apply and combine the components.

```r
> sapply(spIns, sum)
  A   B   C   D   E   F
174 184  25  59  42 200
```

## Another way - plyr package

```r
> library(plyr)
> ddply(InsectSprays, .(spray),summarize,sum=sum(count))
  spray sum
1     A 174
2     B 184
3     C  25
4     D  59
5     E  42
6     F 200
```

What it does is it do “summarize” the “spray” column by doing sum(count)

```r
> sum(InsectSprays$count)
[1] 684

> ave(InsectSprays$count, FUN=sum)
 [1] 684 684 684 684 684 684 684 684 684
[10] 684 684 684 684 684 684 684 684 684
[19] 684 684 684 684 684 684 684 684 684
[28] 684 684 684 684 684 684 684 684 684
[37] 684 684 684 684 684 684 684 684 684
[46] 684 684 684 684 684 684 684 684 684
[55] 684 684 684 684 684 684 684 684 684
[64] 684 684 684 684 684 684 684 684 684

> apraySums <- ddply(InsectSprays,  (spray),summarize,sum=ave(count, FUN=sum))
```
This will sum the count variables by the spray type (A, B and …)

In other words, instead of having a dataset where you just see the sum for A once, you see the sum for A for every value of A in the dataset.

```r
> dim(apraySums)
[1] 72  2

> dim(InsectSprays)
[1] 72  2
```
So the apraySums variable has exactly the same dimention as the passed data set “InsectSprays” 

```r
> head(apraySums)
  spray sum
1     A 174
2     A 174
3     A 174
4     A 174
5     A 174
6     A 174
```

## See also the functions


| Command  | Details |
| ------------ | ------------ |
| acast  | for casting as multi-dimentional arrays  
|  arrange | for faster reordering without using order() commands  |
| mutate  | for adding new variables  |

Credit: Jeff Leek, Assistant Professor of Biostatistics at the Johns Hopkins Bloomberg School of Public Health
