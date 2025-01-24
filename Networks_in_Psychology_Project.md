Networks in Psychology Project
================
Bhuvanesh Wadhwani
2023-02-16

# Loading necessary libraries

``` r
packages_to_use<- c("tidyverse", "igraph", "igraphdata", "qgraph", "influenceR") #packages to use. if installed, load. if not installed, install then load. 

for(i in packages_to_use){
  if( ! i %in% rownames(installed.packages())  ) {
    print(paste(i, "not installed; installing now:\n") )
    install.packages(i)
  }
  
  require(i, character.only = TRUE)
}
```

    ## Loading required package: tidyverse

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
    ## Loading required package: igraph
    ## 
    ## 
    ## Attaching package: 'igraph'
    ## 
    ## 
    ## The following objects are masked from 'package:lubridate':
    ## 
    ##     %--%, union
    ## 
    ## 
    ## The following objects are masked from 'package:dplyr':
    ## 
    ##     as_data_frame, groups, union
    ## 
    ## 
    ## The following objects are masked from 'package:purrr':
    ## 
    ##     compose, simplify
    ## 
    ## 
    ## The following object is masked from 'package:tidyr':
    ## 
    ##     crossing
    ## 
    ## 
    ## The following object is masked from 'package:tibble':
    ## 
    ##     as_data_frame
    ## 
    ## 
    ## The following objects are masked from 'package:stats':
    ## 
    ##     decompose, spectrum
    ## 
    ## 
    ## The following object is masked from 'package:base':
    ## 
    ##     union
    ## 
    ## 
    ## Loading required package: igraphdata
    ## 
    ## Loading required package: qgraph
    ## 
    ## Loading required package: influenceR
    ## 
    ## 
    ## Attaching package: 'influenceR'
    ## 
    ## 
    ## The following objects are masked from 'package:igraph':
    ## 
    ##     betweenness, constraint

# Take in data

``` r
gossip.dat <- read.csv("data_final.csv", header = TRUE, row.names = 1) #loading gossip file that contains negative and positive gossip data
```

# Negative Gossip

``` r
ng.dat <- gossip.dat %>%
  select(NegGosF1R:NegGosM21R) #selecting columns for negative gossip from file

head(ng.dat) #check if the correct columns were selected
```

    ##       NegGosF1R NegGosF2R NegGosF3R NegGosF4R NegGosF5R NegGosF6R NegGosF7R
    ## 10001         0         1         0         0         0         0         1
    ## 10002         1         0         0         0         0         0         1
    ## 10003         0         1         0         1         0         0         0
    ## 10004         0         0         1         0         0         0         0
    ## 10005         0         0         0         1         0         0         1
    ## 10006         0         0         0         0         0         0         1
    ##       NegGosF8R NegGosF9R NegGosF10R NegGosF11R NegGosF12R NegGosF13R
    ## 10001         0         0          0          0          0          0
    ## 10002         0         1          0          0          0          1
    ## 10003         0         0          0          0          0          0
    ## 10004         0         0          0          0          1          0
    ## 10005         0         0          0          0          0          0
    ## 10006         0         0          0          0          1          0
    ##       NegGosF14R NegGosF15R NegGosF16R NegGosF17R NegGosF18R NegGosF19R
    ## 10001          0          0          0          0          0          0
    ## 10002          0          0          0          0          0          1
    ## 10003          0          0          1          0          0          0
    ## 10004          0          0          0          0          0          0
    ## 10005          0          0          1          0          0          1
    ## 10006          1          0          0          0          0          0
    ##       NegGosF20R NegGosF21R NegGosF22R NegGosF23R NegGosF24R NegGosM1R
    ## 10001          1          1          0          1          0         0
    ## 10002          0          0          0          0          0         0
    ## 10003          0          0          0          0          0         0
    ## 10004          0          0          0          0          0         0
    ## 10005          0          0          1          1          0         0
    ## 10006          0          0          0          0          0         0
    ##       NegGosM2R NegGosM3R NegGosM4R NegGosM5R NegGosM6R NegGosM7R NegGosM8R
    ## 10001         0         0         0         0         0         0         0
    ## 10002         0         0         0         1         0         1         0
    ## 10003         0         0         0         0         0         0         0
    ## 10004         0         0         0         0         0         0         0
    ## 10005         0         1         0         0         0         0         0
    ## 10006         0         1         0         0         0         0         0
    ##       NegGosM9R NegGosM10R NegGosM11R NegGosM12R NegGosM14R NegGosM15R
    ## 10001         0          0          0          0          0          0
    ## 10002         0          1          0          1          0          0
    ## 10003         0          0          0          1          0          0
    ## 10004         0          0          0          1          0          0
    ## 10005         0          0          0          0          0          0
    ## 10006         0          0          0          1          0          0
    ##       NegGosM16R NegGosM17R NegGosM18R NegGosM19R NegGosM20R NegGosM21R
    ## 10001          0          1          0          0          0          0
    ## 10002          0          0          0          0          0          0
    ## 10003          0          1          0          0          0          0
    ## 10004          1          1          0          0          1          0
    ## 10005          0          0          0          0          0          0
    ## 10006          0          0          0          0          0          0

``` r
ng.mat <- as.matrix(ng.dat) #convert to matrix
ng.am <- graph_from_adjacency_matrix(ng.mat, mode = "undirected") #convert into igraph object and make sure it is undirected. It should be unweighted by default, so no code needed to run that. 
```

    ## Warning: The `adjmatrix` argument of `graph_from_adjacency_matrix()` must be symmetric
    ## with mode = "undirected" as of igraph 1.6.0.
    ## ℹ Use mode = "max" to achieve the original behavior.
    ## This warning is displayed once every 8 hours.
    ## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
    ## generated.

``` r
ng.am <- simplify(ng.am) #making sure there is only 1 edge between each pair of nodes to remove instances of A -> B and B -> A counting as 2 edges rather than 1. 
is_simple(ng.am) #check simplify function
```

    ## [1] TRUE

``` r
summary(ng.am) #summary of igraph object
```

    ## IGRAPH 5049e50 UN-- 44 264 -- 
    ## + attr: name (v/c)

``` r
#44 nodes, 264 edges
```

# Visualization Negative Gossip

``` r
par(mar=c(0,0,0,0)+.1)  #reduce borders
set.seed(111) #set seed for the same graph layout for reproducability

#plot negative gossip for visualization
plot(ng.am, layout = layout_with_graphopt(ng.am), #layout to view the graph
     vertex.frame.color = 'black', 
     vertex.label.family = 'sans', #readable font
     vertex.size = 8, 
     vertex.color = 'white', 
     vertex.label.cex = 0.7)
```

![](PL4246_Project_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

# Positive Gossip

``` r
pg.dat <- gossip.dat %>%
  select(PosGosF1R:PosGosM21R) #selecting columns for positive gossip. 

head(pg.dat) #check if the correct columns were selected
```

    ##       PosGosF1R PosGosF2R PosGosF3R PosGosF4R PosGosF5R PosGosF6R PosGosF7R
    ## 10001         0         0         1         0         0         0         0
    ## 10002         1         0         1         1         1         1         1
    ## 10003         0         1         0         0         0         0         0
    ## 10004         1         1         1         0         1         0         1
    ## 10005         1         0         1         0         0         0         0
    ## 10006         0         0         0         0         0         0         0
    ##       PosGosF8R PosGosF9R PosGosF10R PosGosF11R PosGosF12R PosGosF13R
    ## 10001         1         0          0          0          0          0
    ## 10002         1         1          1          1          1          1
    ## 10003         1         0          0          0          1          0
    ## 10004         1         1          1          1          0          1
    ## 10005         1         0          1          1          0          0
    ## 10006         1         0          0          0          1          0
    ##       PosGosF14R PosGosF15R PosGosF16R PosGosF17R PosGosF18R PosGosF19R
    ## 10001          1          0          0          0          1          0
    ## 10002          1          1          1          1          1          1
    ## 10003          0          0          1          0          1          0
    ## 10004          0          1          1          1          1          1
    ## 10005          0          0          0          0          0          0
    ## 10006          0          1          1          0          0          0
    ##       PosGosF20R PosGosF21R PosGosF22R PosGosF23R PosGosF24R PosGosM1R
    ## 10001          0          0          0          0          0         0
    ## 10002          1          1          1          1          1         1
    ## 10003          1          0          1          0          0         0
    ## 10004          1          0          1          1          1         0
    ## 10005          0          0          1          0          0         0
    ## 10006          1          1          0          0          0         1
    ##       PosGosM2R PosGosM3R PosGosM4R PosGosM5R PosGosM6R PosGosM7R PosGosM8R
    ## 10001         0         0         0         0         0         0         0
    ## 10002         1         1         1         1         1         1         1
    ## 10003         0         0         0         0         1         1         0
    ## 10004         0         0         0         0         0         1         0
    ## 10005         0         0         0         0         0         1         0
    ## 10006         1         0         0         1         0         0         0
    ##       PosGosM9R PosGosM10R PosGosM11R PosGosM12R PosGosM14R PosGosM15R
    ## 10001         0          0          0          1          0          0
    ## 10002         1          1          1          1          1          1
    ## 10003         0          0          0          0          0          0
    ## 10004         0          0          0          1          0          0
    ## 10005         0          0          0          0          0          0
    ## 10006         0          0          0          1          0          0
    ##       PosGosM16R PosGosM17R PosGosM18R PosGosM19R PosGosM20R PosGosM21R
    ## 10001          0          0          0          0          0          0
    ## 10002          1          1          1          1          1          1
    ## 10003          0          0          0          1          0          0
    ## 10004          0          0          0          1          0          0
    ## 10005          0          0          0          0          0          0
    ## 10006          0          0          1          0          0          1

``` r
pg.mat <- as.matrix(pg.dat) #convert to matrix
pg.am <- graph_from_adjacency_matrix(pg.mat, mode = "undirected") #convert into igraph object and make sure it is undirected. It should be unweighted by default, so no code needed to run that.

pg.am <- simplify(pg.am) #making sure there is only 1 edge between each pair of nodes to remove instances of A -> B and B -> A counting as 2 edges rather than 1.
is_simple(pg.am) #check simplify function
```

    ## [1] TRUE

``` r
summary(pg.am) #summary of igraph object
```

    ## IGRAPH 5073fa0 UN-- 44 475 -- 
    ## + attr: name (v/c)

``` r
#44 nodes, 475 edges
```

# Visualization Positive Gossip

``` r
par(mar=c(0,0,0,0)+.1) #reduce borders
set.seed(111) #set seed for reproducability 

plot(pg.am, layout = layout_with_graphopt(pg.am), 
     vertex.frame.color = 'black',
     vertex.label.family = 'sans',
     vertex.size = 8, 
     vertex.color = 'white', 
     vertex.label.cex = 0.7)
```

![](PL4246_Project_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

# Key Player Analyses

``` r
set.seed(111) #set seed for reproducability  
keyplayer_ng <- keyplayer(ng.am, k = 4) #KPP-Pos. selecting 4 key players from 44 nodes from negative gossip network. 4 was chosen as an estimated number. 
print(keyplayer_ng) #show the 4 key players negative gossip network
```

    ## + 4/44 vertices, named, from 5049e50:
    ## [1] NegGosF12R NegGosF23R NegGosM3R  NegGosM12R

``` r
#F12, F23, M3, M12


set.seed(111) #set seed for reproducability
keyplayer_pg <- keyplayer(pg.am, k = 4) #KPP-Pos. same as above, but for positive gossip network. 4 was chosen again as an estimated number
print(keyplayer_pg) #show the 4 key players in positive gossip network
```

    ## + 4/44 vertices, named, from 5073fa0:
    ## [1] PosGosF12R PosGosF16R PosGosF17R PosGosM2R

``` r
#F12, F16, F17, M2
```

# Visualization negative gossip key player analysis

``` r
# color nodes with highest scores 
V(ng.am)$color <- 'skyblue' #node color
V(ng.am)[keyplayer_ng]$color <- 'limegreen' #select different color for key players to view in the graph 

par(mar=c(0,0,0,0)+.1) #reduce borders

plot(ng.am, layout = layout_with_graphopt(ng.am), 
     vertex.frame.color = 'black', 
     vertex.label.family = 'sans',
     vertex.size = 8, 
     vertex.color = V(ng.am)$color, 
     vertex.label.cex = 0.8,
     vertex.label.color = 'black')
```

![](PL4246_Project_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

# Visualization positive gossip key player analysis

``` r
# color nodes with highest scores 
V(pg.am)$color <- 'skyblue' #node color
V(pg.am)[keyplayer_ng]$color <- 'limegreen' #select different colors for key players to view in the graph  

par(mar=c(0,0,0,0)+.1) #reduce borders

plot(pg.am, layout = layout_with_graphopt(pg.am), 
     vertex.frame.color = 'black', 
     vertex.label.family = 'sans',
     vertex.size = 7, 
     vertex.color = V(pg.am)$color, 
     vertex.label.cex = 0.8,
     vertex.label.color = 'black')
```

![](PL4246_Project_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

# Effective Network Size (ENS) Negative Gossip

``` r
ens_data_ng <- data.frame(node = V(ng.am)$name,
                            ens_score_ng = ens(ng.am)) #ens scores for negative gossip. Getting ens scores (code: ens(ng.am)) with node name (code: V(ng.am)$name), and putting it into a data frame. 

# top 4 ens nodes 
arrange(ens_data_ng, -ens_score_ng) %>% head(4) #filter in descending order (code: -ens_score_ng) and show the top 4 nodes with the highest ens scores (code: head(4)) 
```

    ##                  node ens_score_ng
    ## NegGosM12R NegGosM12R     18.23077
    ## NegGosF12R NegGosF12R     15.57143
    ## NegGosF21R NegGosF21R     14.90000
    ## NegGosM3R   NegGosM3R     14.47368

``` r
#M12, F12, F21, M3
```

# Visualization Negative Gossip ENS

``` r
# color nodes with highest ENSes 
V(ng.am)$color <- 'skyblue'
V(ng.am)[name %in% c('NegGosM12R', 'NegGosF12R', 'NegGosF21R', 'NegGosM3R')]$color <- 'green' #highest ens nodes color

par(mar=c(0,0,0,0)+.1)  #reduce borders

plot(ng.am, 
     layout = layout_with_graphopt(ng.am), 
     vertex.frame.color = 'black', 
     vertex.label.family = 'sans',
     vertex.size = 8, 
     vertex.color = V(ng.am)$color, 
     vertex.label.cex = 0.6,
     vertex.label.color = 'black')
```

![](PL4246_Project_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

# Effective Network Size (ENS) Positive Gossip

``` r
ens_data_pg <- data.frame(node = V(pg.am)$name,
                            ens_score_pg = ens(pg.am)) #ens scores for positive gossip. Getting ens scores (code: ens(pg.am)) with node name (code: V(pg.am)$name), and putting it into a data frame.

# top 4 ens nodes
arrange(ens_data_pg, -ens_score_pg) %>% head(4) #filter in descending order (code: -ens_score_pg) and show the top 4 nodes with the highest ens scores (code: head(4))
```

    ##                  node ens_score_pg
    ## PosGosF2R   PosGosF2R     22.90698
    ## PosGosF12R PosGosF12R     19.76923
    ## PosGosM12R PosGosM12R     17.33333
    ## PosGosF7R   PosGosF7R     13.50000

``` r
#F2, F12, M12, F7
```

# Visualization Positive Gossip ENS

``` r
# color nodes with highest ENSes 
V(pg.am)$color <- 'skyblue'
V(pg.am)[name %in% c('PosGosF2R', 'PosGosF12R', 'PosGosM12R', 'PosGosF7R')]$color <- 'green' #highest ens nodes color

par(mar=c(0,0,0,0)+.1) 

plot(pg.am, 
     layout = layout_with_graphopt(pg.am), 
     vertex.frame.color = 'black', 
     vertex.label.family = 'sans',
     vertex.size = 8, 
     vertex.color = V(pg.am)$color, 
     vertex.label.cex = 0.6,
     vertex.label.color = 'black')
```

![](PL4246_Project_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

# Centrality measures for negative gossip network

``` r
degree(ng.am) |> sort(decreasing = T) |> head(4) #top 4 node degree in negative gossip network. Done by sorting in decreasing order (code: sort(decreasing = T) then showing top 4 scores (code: head(4)). 
```

    ## NegGosM12R NegGosF12R  NegGosM5R NegGosF21R 
    ##         26         21         21         20

``` r
closeness(ng.am)|> sort(decreasing = T) |> head(4) #top 4 node closeness in negative gossip network.Done by sorting in decreasing order (code: sort(decreasing = T) then showing top 4 scores (code: head(4)). 
```

    ## NegGosM12R NegGosF12R  NegGosM5R NegGosF21R 
    ## 0.01666667 0.01538462 0.01538462 0.01515152

``` r
#Top 4 for both degree and closeness = M12, F12, M5, F21. Both showed the same 4.
```

# Centrality measures for positive gossip network

``` r
degree(pg.am) |> sort(decreasing = T) |> head(4) #top 4 node degree in positive gossip network. Done by sorting in decreasing order (code: sort(decreasing = T) then showing top 4 scores (code: head(4)). 
```

    ##  PosGosF2R PosGosF12R PosGosM12R  PosGosM7R 
    ##         43         39         36         30

``` r
closeness(pg.am)|> sort(decreasing = T) |> head(4) #top 4 node closeness in positive gossip network. Done by sorting in decreasing order (code: sort(decreasing = T) then showing top 4 scores (code: head(4)). 
```

    ##  PosGosF2R PosGosF12R PosGosM12R  PosGosM7R 
    ## 0.02325581 0.02127660 0.02000000 0.01785714

``` r
#Top 4 for both degree and closeness = F2, F12, M12, M7. Both showed the same 4.
```
