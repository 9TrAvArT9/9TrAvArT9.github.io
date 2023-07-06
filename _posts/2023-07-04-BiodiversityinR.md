---
layout: post
title: Analyzing Biodiversity in R
subtitle: How R Makes it Easy to Understand Biodiversity 
date: '2023-07-04 12:47:00 -08:00'
background: '/img/posts/biodiversity.png'
full-width: true
---

<style type="text/css">
  p {
    width: 900px;
  }
</style>

## What is Biodiversity?

Biological diversity, or *biodiversity*, is the variety and variability of life on Earth, along with their interactions. As a central topic in Ecology, biodiversity can be examined from the genetic to the ecosystem level. Organisms are intrinsically linked, so changes in the life cycle of one species indelibly affects the life cycles of many others. Hence, biodiversity can be an indicator of an ecosystem's overall health and of primary interest to an Ecologist. But how can we measure biodiversity, or assess the validity of any proposed metric?

Although an intuitive concept, precisely defining and assessing biodiversity can be a difficult task. Generally speaking, there are three primary levels of biodiversity - genetic, species, and ecosystem. These can be assessed and measured as phylogenetic, taxonomic, and functional diversity, respectively. 

Genetic diversity refers to the variety of genotypes within an assemblage. It usually pertains to the range of inherited traits *within* a particular species. This variation in genetic material result in different traits of individuals, which in turn affect an individuals' interaction with their surrounding environment. *Phylogenetic diversity* refers to the genetic diversity within a set of species or ecosystem. It is measured as the sum of the length of all the branches of a phylogenetic tree that span the members of the set. Hence, we can consider the genetic diversity *within* a species, and the phylogenetic diversity *between* species in an assemblage.

Species, or taxonomic diversity refers to the variety of species or taxa in a given assemblage. This is the level of diversity most commonly thought of when one thinks of biodiversity. Taxonomic diversity is more precisely thought of as a measure of the richness and abundance of species. Generally, a high variety of species types (high richness) coupled with relatively even proportions of each (relative abundance) is considered to have a large measure of taxonomic diversity. Areas of high biodiversity are typically given precedence in conservation efforts. 

Ecosystem diversity is a broad view of biodiversity. It considers entire populations of species along with the dynamic interactions with their environments (local geography, natural resources, etc...). Loss of genetic or species diversity will affect the performance of ecosystems. Scientists generally assess diversity at this level as *functional diversity*, or trait diversity. This generally concerns the range of things that organisms actually do in communities or ecosystems. It is measured as the diversity of the value and range of species traits that are important to the structure and dynamic stability of the community. For instance, species distribution, growth forms, or resource use strategy are several components that may be considered when assessing functional diversity. 

We can also think of the *alpha* (within) and *beta* (between) diversities in a system. Phylogenetic, taxonomic, and functional diversity are typically measured as alpha components. Species beta diversity can be completely decomposed into *species nestedness* and *species turnover*. Nestedness is a measure of structure within an ecological system, usually applied to describe the distribution of species across locations, or the interactions between species. Turnover refers to the rate or magnitude of change in species composition along spatial or environmental gradients. There are a numbers of established measures associated with each type of biodiversity, but the true biodiversity of a given system is the realization of a complex interaction of all of these. 

## How Do We Measure Biodiversity?

When Biologists first started talking about diversity, they were simply referring to the number of species in a community, also known as the *species richness*. It was soon realized in practice that accurately measuring species richness is a time consuming and difficult task. Indeed, it is nearly impossible to capture or observe *every* type of species in a community, particularly in areas with high richness, where many species are rare. Despite this, **Species Richness** is still an exceedingly important parameter in biodiversity, and is often used to prioritize land usage. 

There are many applications where in which a simple (lower-bound) estimate of species count is insufficient. After all, a forrest of pine trees with one maple, is ecologically quite different from a maple forest with one pine. Hence, Biologists needed to incorporate *relative abundance* into diversity metrics. The idea being that diversity is maximized when all species are equally common, and minimum when all but one species are exceedingly rare, decreasing as species head to extinction. Considering these features, and borrowing from concepts in Economics, the *Pigou-Dalton principle of transfer* was introduced to biological systems. The notion being, for a fixed number of individuals, diversity should increase when abundance is transferred from one species to another strictly rarer species, or when any new vanishingly rare species is added. All of this culminated into a new diversity measure called the **Gini-Simpson Index**, which is calculated as the probability that when two individuals randomly encounter each other, they be different species. The probability of an inter-specific encounter obeys the principle of transfer and can be used to measure the compositional complexity of an ecosystem. 

Another property of an ecosystem that behaves similarly is the probability of the species identity of an individual in a random draw from the community. This probability can be estimated from the relative species abundance by using information theory and is just the **Shannon entropy**. Biologists refer to this as the *Shannon-Weiner Index* and has often been used as a diversity metric. {::nomarkdown}  
\begin{equation}
\textbf{Species Richness:}\ \ \ \sum_{i=1}^S p_i\\
\textbf{Gini-Simpson:}\ \ \ 1 - \sum_{i=1}^Sp_i^2 \ \ \ \ \ \ \ \ \ \ \ \ \textbf{Shannon entropy:}\ \ \ -\sum_{i=1}^Sp_i ln p_i\\
\end{equation}
\begin{align} \textit{Where each species is indexed $i = 1,2,\cdots, S$ with relative abundance $p_i$} \end{align}
{:/}  
                                            
Each of these three diversity have quite different units and mathematical properties, so cannot be directly compared with each other. Additionally, comparisons and ratios of the same index could be misleading, as both the Shannon and Simpson indices are non-linear. In the late 1990s, master formulas that generated each of these diversity indices began appearing from varying disciplines. A key breakthrough came from Ecologist Mark Hill, who realized that all these master formulas could be transformed so that they generate a family of measures with an easily interpretable metric. 

### Hill Numbers 

{::nomarkdown}
\begin{equation}
\textbf{Hill Numbers:}\\
\textit{When $q > 0$ and $q\neq 1$}\ \ \ \ \ \ \ \ {}^{q}D = \Bigg(\sum_{i=1}^S p_i^q\Bigg)^{1/(1-q)}\\
\text{} \\
\text{When q = 1}\ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ {}^{1}D = exp\Big(-\sum_{i=1}^Sp_i ln p_i\Big) 
\end{equation}
{:/}

**Hill Numbers** are in units of *effective number of speices* and is a function of the parameter $q$. This parameter determines the sensitivity to species relative abundance. The traditional biodiversity metrics can be found with simple transformations and by setting $q$ with *Species Richness (q=0), Gini-Simpson (q=1)*, and *Shannon entropy (q=2)*. It was found that this measurement of diversity obey another principle key in Economics - the *replication principle*: if we pool $N$ equally large, equally diverse communities with NO shared species, the diversity of this pooled community should be $N$ times the diversity of a single community. This now solves the previous problems of ratios and comparisons with the Shannon entropy and Gini-Simpsons indices, and supports the rules of inference used by Biologists. In addition to species richness and relative abundance, Hill numbers have recently been generalized to incorporate phylogentic distances as well. Hill numbers, or the effective number of species best quantifies certain concepts of diversity, particularly species and taxonomic. Many of the R packages commonly used for diversity analysis make direct use of Hill numbers. 

## Assessing Biodiversity in R

To better understand how we can assess diversity in R, let's consider an example with artificial set of data pertaining to bat species in from ten sites in subtropical Africa. Sites 1-6 are geographically close to an inselberg structure, while sites 7-10 are distant, but close to each other. Our goal is to compare metrics of diversity between the two transects - one NEAR the inselberg, and another DISTANT from the inselberg. At each site, bats were captured, tagged, and number of variables recorded. Here's a glimpse at some of the data...

![](/img/posts/batdata.png){: width="700" style="display:block; margin-left:auto; margin-right:auto"}

Here we can observe six of the 36 species used in this study, along with several variables pertaining to functional diversity, such as Foraging Mode, and Roosting. Don't worry about how to interpret any particular scores in this data set just yet. Instead, let's begin by examining Taxonomic diversity between the transects. 

### Taxonomic Diversity 

**iNEXT** (INterpolation and EXTrapolation) is a package in R that provides simple functions to compute estimates of Hill numbers with their associated R/E (rarefaction/extrapolation) curves. *Abundance* data, a type of biodiversity data, is used by iNEXt to make these calculations. This is essentially a list of the total count of each species observed, and accepted as a matrix input. A glimpse at the abundance matrix of our total sample data is shown below. 

![](/img/posts/abuntable1.png){: width = "400" style="display:block; margin-left:auto; margin-right:auto"}

The abundance matrix used in this study contains the species counts for all species found in each transect - NEAR and FAR. iNEXT is conveniently equipped with the ggiNEXT() function, which is an extension of the familiar and loved ggplot2 package. We begin by plotting a sample-size based R/E curve. These types of R/E curves computes diversity estimates for rarefied and extrapolated samples up to double the reference sample. 

```{r}
library(iNEXT);
#Pool data
dataNEAR <- data %>% 
              filter(Transect %in% 'NEAR');
dataFAR <- data %>% 
              filter(Transect %in% 'FAR');
#Abundance Matrices
spec.tbl.NEAR <- as.data.frame(table(dataNEAR$Species));
abund.NEAR <- sort(c(spec.tbl.NEAR$Freq), decreasing = T);

spec.tbl.FAR <- as.data.frame(table(dataFAR$Species));
abund.FAR <- sort(c(spec.tbl.FAR$Freq), decreasing = T);
#Concatenate as list
abund.inselberg <- list(abund.NEAR,abund.FAR);
names(abund.inselberg) <- c("NEAR","FAR");

#Establish the R/E bootstrapped distribution
#By default, q = 0 

inselberg.dist <- iNEXT(abund.total, q = c(0,1,2), datatype = 'abundance');
#Type = 1 is a sample-size based R/E curve, se=TRUE displays 95% confidence bands
ggiNEXT(inselberg.dist, type = 1, se = TRUE)
```
Which can be formatted to display the following plot
![](/img/posts/REexample.png){: style="display:block; margin-left:auto; margin-right:auto" }

We can now easily compare the three common metrics of taxonomic diversity. From the plots, we observe estimated measures to be consistently larger in the 'NEAR' inselberg data. However, these findings are not deemed statistically significant due to the apparent overlap in the bootstrapped 95% confidence bands. 

In addition to we can plot coverage-based R/E curves, which calculates diversity estimates for rarefied and extrapolated samples with sample completeness (sample coverage). This way we can compare equally-complete samples, since Hill number estimates are sensitive to sampling coverage.  

```{r}
#Type = 3 for coverage-based R/E curve
ggiNEXT(inselberg.dist, type = 3, se = TRUE)
```

![](/img/posts/smplcoverage.png){: style="display:block; margin-left:auto; margin-right:auto" }

These plots allow us to understand the nature of our estimates as our sampling coverage becomes more complete. As mentioned, complete sampling coverage is nearly impossible, particularly in species rich areas. To link the sample-sized based and coverage-based sampling curves, it is informative to examine the sample completeness curve as follows.

```{r}
#Type = 2 for sampling completness
ggiNEXT(inselberg.dist, type = 2, se = TRUE)
```

![](/img/posts/sampcompl.png){: style="display:block; margin-left:auto; margin-right:auto" height = "100"}

The nature of these curves tell us the ecoregion near the inselberg has greater potential for numerous individuals. 

The iNEXT package also has convenient functions for tabular display of output information from the  iNEXT object. 

```{r}
inselberg.dist$AsyEst
```

![](/img/posts/esttable.png){: style="display:block; margin-left:auto; margin-right:auto"}

### Functional Diversity 

Functional diversity is based on the use of functional species traits which are defined as biological attributes that influence organismal performance. This aims at taking a functional approach to community ecology, *independent of taxonomy*. A step beyond species richness, functional diversity is a powerful tool to link community composition to ecosystem properties. Typically, we split functional diversity into three independent facets - Richness, Evenness, and Divergence, as proposed by Villéger, et. al. 

Here, our data contain many previously mentioned functional characteristics of our bat species, such as body weight, roosting strategy, and foraging range. The R package **fundiversity** is designed to ease the ease tee computational burden of classical indices of functional diversity. Each function available in this package computes a single index with two main inputs - a species by traits matrix and a site by species matrix. 

A species by traits matrix is structured to have species as the row names and funtional traits as the columns. A site by species matrix is designed to have sites by row and species by column. This is demonstrated below for our data. 

<center>
<figure>
<img src = '/img/posts/sptrt.png' width = "500"/>
<figcaption>Species by Traits</figcaption>
</figure>
</center>
<br>

<center>
<figure>
<img src = '/img/posts/sitesp.png' width = "750">
<figcaption>Site by Species </figcaption>
</figure>
</center>

