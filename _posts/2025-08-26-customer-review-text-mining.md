---
title: "Customer Review Analysis"
format: html
editor: visual
---

# Customer Review Analysis

## 1. Introduction

Customer reviews are very valuable information for business decisions.

We are going to use text mining to extract quantifiable information to use for analysis

## 2. Data Pre-processing

We have to convert the unstructured data into structured format to apply descriptive statistics.


```
##  [1] "building"           "corpus"             "cosine_dist_matrix" "cosine_distance"    "cosine_similarity" 
##  [6] "crs"                "crv"                "d"                  "ddata"              "denominator"       
## [11] "dist_obj"           "dtm"                "dtm_matrix"         "g"                  "groceries_data"    
## [16] "hclust_obj"         "m"                  "numerator"          "p01"                "p02"               
## [21] "p03"                "p04"                "p05"                "p06"                "p07"               
## [26] "reviews"            "rules"              "rules_conf"         "scoring"            "texts"             
## [31] "transactions_data"  "transactions_list"  "v"
```


``` r
dtm <- DocumentTermMatrix(corpus)
inspect(dtm)
```

```
## <<DocumentTermMatrix (documents: 21, terms: 185)>>
## Non-/sparse entries: 254/3631
## Sparsity           : 93%
## Maximal term length: 11
## Weighting          : term frequency (tf)
## Sample             :
##     Terms
## Docs alfredo best chicken deep dish food good great pizza sauce
##   10       1    0       1    0    0    1    0     0     1     0
##   11       0    0       0    0    0    0    0     0     0     1
##   12       0    1       0    1    1    1    1     0     2     0
##   17       0    0       0    1    1    0    0     0     1     0
##   19       0    1       2    1    1    0    0     0     2     0
##   2        0    0       0    0    0    2    1     0     0     0
##   20       1    0       0    0    1    0    1     0     1     0
##   21       0    0       0    0    1    0    1     2     0     0
##   5        0    0       0    0    0    0    0     0     0     2
##   9        1    0       1    0    0    0    1     1     1     1
```


``` r
numerator <- crossprod_simple_triplet_matrix(dtm)
denominator <- sqrt(col_sums(dtm^2)) %*% t(sqrt(col_sums(dtm^2)))
cosine_similarity <- numerator / denominator
cosine_distance <- 1 - cosine_similarity
```


``` r
cosine_dist_matrix <- as.matrix(cosine_distance)
print(round(cosine_dist_matrix, 2))
```

```
##              Terms
## Terms         absolutely  add alfredo also although amazing appetizers atmosphere attitudes bad barely best better butter
##   absolutely        0.00 1.00    0.59 1.00     1.00    1.00       1.00       0.29         1   1   1.00 0.65   0.29   1.00
##   add               1.00 0.00    1.00 1.00     1.00    1.00       1.00       1.00         1   1   1.00 1.00   1.00   1.00
##   alfredo           0.59 1.00    0.00 1.00     0.42    1.00       1.00       1.00         1   1   1.00 1.00   0.42   0.42
##   also              1.00 1.00    1.00 0.00     1.00    1.00       1.00       1.00         1   1   1.00 0.65   1.00   1.00
##   although          1.00 1.00    0.42 1.00     0.00    1.00       1.00       1.00         1   1   1.00 1.00   1.00   0.00
##              Terms
## Terms         caccatore called cheese cheeses chew chicago chicken choose classic comes complaints cooked creamy crepe cute  day
##   absolutely       1.00   1.00   1.00    1.00 1.00    1.00    1.00   1.00    1.00  1.00       1.00   1.00   1.00  1.00 0.29 1.00
##   add              1.00   1.00   0.29    1.00 1.00    1.00    1.00   1.00    1.00  0.00       1.00   1.00   1.00  1.00 1.00 1.00
##   alfredo          1.00   1.00   1.00    1.00 1.00    1.00    0.53   1.00    1.00  1.00       1.00   1.00   1.00  1.00 1.00 1.00
##   also             1.00   0.29   1.00    1.00 1.00    0.29    1.00   1.00    1.00  1.00       1.00   1.00   1.00  1.00 1.00 1.00
##   although         1.00   1.00   1.00    1.00 1.00    1.00    0.59   1.00    1.00  1.00       1.00   1.00   1.00  1.00 1.00 1.00
##              Terms
## Terms         deep definite definitely delicious deterrent diamond  die dish dont  dry entire ever excellent eyes famous
##   absolutely  1.00     1.00       1.00      1.00      1.00    1.00 1.00 0.68 1.00 1.00   1.00 1.00      1.00 1.00   1.00
##   add         1.00     1.00       0.00      1.00      1.00    1.00 1.00 0.55 0.00 1.00   0.00 1.00      1.00 1.00   1.00
##   alfredo     1.00     1.00       1.00      1.00      1.00    1.00 1.00 0.74 1.00 0.42   1.00 1.00      1.00 1.00   1.00
##   also        0.59     1.00       1.00      1.00      1.00    1.00 1.00 0.68 1.00 1.00   1.00 0.50      1.00 1.00   1.00
##   although    1.00     1.00       1.00      1.00      1.00    1.00 1.00 1.00 1.00 1.00   1.00 1.00      1.00 1.00   1.00
##              Terms
## Terms         fantastic fantastico favorite fettuccine fighting filthy five flavorful food fooddo forever fourty fresh friend
##   absolutely       0.29       1.00     1.00       0.29     1.00   1.00 1.00      1.00 0.76   1.00    1.00   1.00  1.00   1.00
##   add              1.00       1.00     1.00       1.00     1.00   1.00 1.00      1.00 1.00   1.00    1.00   1.00  1.00   1.00
##   alfredo          1.00       1.00     1.00       0.42     1.00   1.00 1.00      0.42 0.81   1.00    1.00   1.00  0.59   1.00
##   also             1.00       1.00     1.00       1.00     0.29   1.00 0.50      1.00 0.53   1.00    1.00   0.29  1.00   1.00
##   although         1.00       1.00     1.00       1.00     1.00   1.00 1.00      0.00 0.67   1.00    1.00   1.00  0.29   1.00
##              Terms
## Terms         friendly garden garlic gnocchi going good  got great happy heard hiking home homemade house however huge including
##   absolutely      1.00   0.29   1.00    1.00     1 0.79 1.00  0.75  1.00  1.00   1.00 1.00     1.00  1.00    1.00 1.00      1.00
##   add             1.00   1.00   1.00    1.00     1 0.70 1.00  0.29  1.00  1.00   1.00 1.00     1.00  1.00    1.00 1.00      1.00
##   alfredo         1.00   0.42   0.42    1.00     1 0.65 1.00  0.80  1.00  0.42   1.00 1.00     1.00  1.00    0.42 1.00      1.00
##   also            1.00   1.00   1.00    1.00     1 0.36 0.29  0.75  1.00  1.00   1.00 1.00     1.00  0.29    1.00 1.00      1.00
##   although        1.00   1.00   0.00    1.00     1 1.00 1.00  1.00  1.00  1.00   1.00 1.00     1.00  1.00    0.00 1.00      1.00
##              Terms
## Terms         instead italian item  ive just lasagna lasagne left like linguini little  lol long loud lousy love lovers made
##   absolutely     1.00    1.00 1.00 0.29 1.00    1.00    0.29 1.00 1.00     1.00   1.00 0.29 1.00 1.00     1 1.00   1.00 1.00
##   add            1.00    1.00 1.00 1.00 1.00    0.11    1.00 1.00 0.00     1.00   1.00 1.00 1.00 1.00     1 1.00   1.00 0.42
##   alfredo        1.00    1.00 1.00 0.42 0.42    1.00    0.42 1.00 1.00     0.42   1.00 1.00 1.00 1.00     1 1.00   1.00 1.00
##   also           1.00    1.00 1.00 1.00 1.00    1.00    1.00 0.29 1.00     1.00   1.00 1.00 1.00 1.00     1 1.00   1.00 1.00
##   although       1.00    1.00 1.00 1.00 1.00    1.00    1.00 1.00 1.00     0.00   1.00 1.00 1.00 1.00     1 1.00   1.00 1.00
##              Terms
## Terms         make manicotti many meals meat meatballs melt melts menu minute mouth mozarella much mushrooms mussels nice okay
##   absolutely  1.00      1.00 0.50     1 1.00      1.00 1.00  1.00 1.00   1.00  1.00      1.00 1.00      1.00    1.00 1.00 1.00
##   add         1.00      1.00 1.00     1 0.42      1.00 1.00  1.00 1.00   1.00  1.00      1.00 1.00      1.00    1.00 1.00 1.00
##   alfredo     1.00      1.00 0.59     1 1.00      1.00 1.00  1.00 1.00   1.00  1.00      1.00 1.00      1.00    0.42 0.59 0.42
##   also        1.00      1.00 1.00     1 1.00      1.00 1.00  1.00 0.29   0.29  1.00      1.00 1.00      1.00    1.00 0.50 1.00
##   although    1.00      1.00 1.00     1 1.00      1.00 1.00  1.00 1.00   1.00  1.00      1.00 1.00      1.00    0.00 1.00 1.00
##              Terms
## Terms         okive olive options order ordered ordering overpower overrated  pan parm pasta people perfectly pesto  pie pizza
##   absolutely   0.29  0.29    1.00  1.00    1.00     1.00      1.00         1 1.00 1.00  1.00   1.00      1.00  1.00 1.00  0.82
##   add          1.00  1.00    1.00  1.00    1.00     1.00      0.00         1 1.00 1.00  1.00   1.00      1.00  1.00 1.00  1.00
##   alfredo      0.42  0.42    1.00  1.00    0.42     1.00      1.00         1 1.00 1.00  1.00   1.00      1.00  1.00 1.00  0.55
##   also         1.00  1.00    1.00  1.00    1.00     1.00      1.00         1 0.29 1.00  1.00   0.29      1.00  1.00 0.29  0.45
##   although     1.00  1.00    1.00  1.00    0.00     1.00      1.00         1 1.00 1.00  1.00   1.00      1.00  1.00 1.00  0.74
##              Terms
## Terms         pizzas place places plump portions pretty prices ready real really reason reasonable recommend rest ricotta rough
##   absolutely    1.00  1.00   0.29  1.00     1.00   1.00   1.00  1.00 1.00   1.00   1.00       1.00      1.00 1.00    1.00  1.00
##   add           1.00  1.00   1.00  1.00     1.00   1.00   1.00  1.00 1.00   0.42   1.00       1.00      1.00 1.00    0.29  1.00
##   alfredo       1.00  1.00   0.42  1.00     1.00   1.00   1.00  1.00 1.00   0.67   1.00       1.00      1.00 1.00    1.00  1.00
##   also          1.00  1.00   1.00  1.00     1.00   1.00   1.00  0.29 0.29   0.59   1.00       1.00      1.00 0.29    1.00  1.00
##   although      1.00  1.00   1.00  1.00     1.00   1.00   1.00  1.00 1.00   1.00   1.00       1.00      1.00 1.00    1.00  1.00
##              Terms
## Terms         sauce seamlessly seating service shrimp shrimps slow spaghetti special spectacular spices staff stars steamed stop
##   absolutely   1.00       1.00    1.00    1.00   1.00    1.00 1.00      1.00    1.00        1.00   1.00  1.00  1.00    1.00 1.00
##   add          1.00       1.00    1.00    1.00   1.00    1.00 1.00      1.00    1.00        1.00   0.00  1.00  1.00    1.00 1.00
##   alfredo      0.76       1.00    1.00    1.00   1.00    1.00 1.00      1.00    1.00        0.42   1.00  1.00  1.00    0.42 1.00
##   also         1.00       1.00    0.29    0.59   1.00    1.00 1.00      1.00    1.00        1.00   1.00  1.00  1.00    1.00 1.00
##   although     1.00       1.00    1.00    1.00   1.00    1.00 1.00      1.00    1.00        1.00   1.00  1.00  1.00    0.00 1.00
##              Terms
## Terms         stopped stuffed style sublime super tails take takes taste tasted thing time tops tortellini tortellinis tried
##   absolutely        1    1.00  1.00    1.00  1.00  1.00 1.00  1.00  0.50   1.00  1.00 1.00 1.00       1.00        1.00  0.50
##   add               1    1.00  1.00    1.00  1.00  1.00 1.00  1.00  1.00   1.00  1.00 1.00 1.00       1.00        1.00  1.00
##   alfredo           1    1.00  1.00    1.00  1.00  1.00 1.00  1.00  0.18   0.42  0.42 1.00 1.00       1.00        1.00  0.59
##   also              1    1.00  0.29    1.00  1.00  1.00 1.00  1.00  1.00   1.00  1.00 1.00 0.29       1.00        1.00  1.00
##   although          1    1.00  1.00    1.00  1.00  1.00 1.00  1.00  0.29   1.00  1.00 1.00 1.00       1.00        1.00  1.00
##              Terms
## Terms         veggie wait waiter want wasnt watering way white wine worth yummy
##   absolutely    1.00 1.00   0.50 1.00  1.00     1.00   1  1.00 1.00  1.00  1.00
##   add           1.00 1.00   1.00 0.00  1.00     1.00   1  1.00 1.00  1.00  1.00
##   alfredo       1.00 1.00   0.59 1.00  0.42     1.00   1  0.42 0.42  1.00  1.00
##   also          1.00 0.29   1.00 1.00  1.00     1.00   1  1.00 1.00  0.29  1.00
##   although      1.00 1.00   1.00 1.00  1.00     1.00   1  0.00 0.00  1.00  1.00
##  [ reached 'max' / getOption("max.print") -- omitted 180 rows ]
```


``` r
heatmap(cosine_dist_matrix, col = colorRampPalette(c("white", "steelblue"))(100))
```

![plot of chunk heatmap](/assets/text-mining/heatmap-1.png)


``` r
dtm_matrix <- as.matrix(dtm)  # Convert sparse DTM to full matrix
dist_obj <- proxy::dist(dtm_matrix, method = "cosine")  # Proper cosine distance

hclust_obj <- hclust(dist_obj, method = "ward.D2")
plot(hclust_obj, labels = paste("Doc", 1:nrow(dtm_matrix)), main = "Document Clustering")
```

![plot of chunk clustering](/assets/text-mining/clustering-1.png)


``` r
m <- as.matrix(dtm)
v <- sort(colSums(m), decreasing = TRUE)
d <- data.frame(word = names(v), freq = v)


set.seed(123)  # for reproducibility
wordcloud(
  words = d$word,
  freq = d$freq,
  min.freq = 1,
  max.words = 100,
  random.order = FALSE,
  rot.per = 0.35,
  colors = brewer.pal(8, "Dark2")
)
```

![plot of chunk wordcloud](/assets/text-mining/wordcloud-1.png)
