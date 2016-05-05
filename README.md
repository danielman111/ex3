---
title: "HW3"
author: "Daniel Sinaniev and Ben Eyal"
date: "May 4, 2016"
output: pdf_document
---

```{r, echo=FALSE}
library('knitr')
library('igraph')
```

```{r}
ga.data <- read.csv('ga_edgelist.csv', header=TRUE)
g <- graph.data.frame(ga.data, directed=FALSE)
```

# Question 1.a

## Find Biggest Component

```{r}
cl <- clusters(g)
to.keep <- which(cl$membership == which.max(cl$csize))
biggest_comp <- induced.subgraph(g, to.keep)
summary(biggest_comp)
```

## Closeness

```{r}
closeness(biggest_comp)
maxCloseness <- max(closeness(biggest_comp))
maxClosenessName <- V(biggest_comp)$name[closeness(biggest_comp)==max(closeness(biggest_comp))]
maxClosenessName
maxCloseness
```

## Betweeness

```{r}
betweenness(biggest_comp)
maxBetweenness <- max(betweenness(biggest_comp))
maxBetweennessName <- V(biggest_comp)$name[betweenness(biggest_comp)==max(betweenness(biggest_comp))]
maxBetweennessName
maxBetweenness
```

## Eigenvector

```{r}
evcent(biggest_comp)$vector
maxEvcent <- max(evcent(biggest_comp)$vector)
maxEvcentName <- V(biggest_comp)$name[evcent(biggest_comp)$vector==max(evcent(biggest_comp)$vector)]
maxEvcentName
maxEvcent
```

# Question 1.b

## Community Detection - First Algorithm

### Short Random Walks

```{r}
RandomWalk <- walktrap.community(biggest_comp)
lay <- layout.kamada.kawai(biggest_comp)
RandomWalkMemb <- community.to.membership(biggest_comp, RandomWalk$merges, which.max(RandomWalk$modularity))
RandomWalkMemb
#num of communities = 6
#the sizes are 11  2  2  3  3  3
RandomWalk$modularity
plot(biggest_comp, layout=lay, vertex.size=5, vertex.color=RandomWalkMemb$membership+1,
     asp=FALSE)
```

### Girvan-Newman

```{r}
GirvanNewman <-  edge.betweenness.community(biggest_comp)
GirvanNewmanMemb <- community.to.membership(biggest_comp, GirvanNewman$merges, which.max(GirvanNewman$modularity))
GirvanNewmanMemb
#num of communities = 5
#the sizes are 8 5 4 4 3
plot(biggest_comp, layout=lay, vertex.size=5, vertex.color=GirvanNewmanMemb$membership+1, asp=FALSE)
```

# Question 2

The file dolphins.gml contains an undirected social network of
frequent associations between 62 dolphins in a community living off
Doubtful Sound, New Zealand, as compiled by Lusseau et al. (2003)

```{r}
dolphinesNet <- read.graph('dolphines.gml',format = "gml")
```

## Find Biggest Component

```{r}
cl <- clusters(dolphinesNet)
to.keep <- which(cl$membership == which.max(cl$csize))
Dolphines_big_comp <- induced.subgraph(dolphinesNet, to.keep)
summary(Dolphines_big_comp)
```

## Closeness

```{r}
closeness(Dolphines_big_comp)
maxCloseness <- max(closeness(Dolphines_big_comp))
maxClosenessName <- V(Dolphines_big_comp)$label[closeness(Dolphines_big_comp)==max(closeness(Dolphines_big_comp))]
maxClosenessName
maxCloseness
```

## Betweeness

```{r}
betweenness(Dolphines_big_comp)
maxBetweenness <- max(betweenness(Dolphines_big_comp))
maxBetweennessName <- V(Dolphines_big_comp)$label[betweenness(Dolphines_big_comp)==max(betweenness(Dolphines_big_comp))]
maxBetweennessName
maxBetweenness
```

## Eigenvector

```{r}
evcent(Dolphines_big_comp)$vector
maxEvcent <- max(evcent(Dolphines_big_comp)$vector)
maxEvcentName <- V(Dolphines_big_comp)$label[evcent(Dolphines_big_comp)$vector==max(evcent(Dolphines_big_comp)$vector)]
maxEvcentName
maxEvcent
```

