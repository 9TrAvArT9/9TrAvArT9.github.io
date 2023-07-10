---
layout: post
title: Analyzing Biodiversity in R
subtitle: How R Makes it Easy to Assess Biodiversity 
date: '2023-07-04 12:47:00 -08:00'
background: '/img/posts/biodiversity.png'
full-width: true
---

<style type="text/css">
  p {
    width: 900px;
  }∏
</style>

## What is Biodiversity?

Biological diversity, or *biodiversity*, is the variety and variability of life on Earth, along with their interactions. As a central topic in Ecology, biodiversity can be examined from the genetic to the ecosystem level. Organisms are intrinsically linked, so changes in the life cycle of one species indelibly affects the life cycles of many others. Hence, biodiversity can be an indicator of an ecosystem's overall health and of primary interest to an Ecologist. But how can we measure biodiversity, or assess the validity of any proposed metric?

Although an intuitive concept, precisely defining and evaluating diversity can be a difficult task. Generally speaking, there are three primary levels of biodiversity - genetic, species, and ecosystem. These can be assessed and measured as phylogenetic, taxonomic, and functional diversity, respectively. 

Genetic diversity refers to the variety of genotypes within an assemblage. It usually pertains to the range of inherited traits *within* a particular species. This variation in genetic material result in differing attributes of individuals, which in turn affect interactions with the surrounding environment. *Phylogenetic diversity* refers to the genetic diversity within a *set* of species or ecosystem. It is quantified as the sum of the lengths of all the branches of the phylogenetic tree that span all members of the given set. Hence, we can usually consider the genetic diversity *within* a species, and the phylogenetic diversity *between* species in an assemblage.

Species, or taxonomic diversity refers to the variety of species or taxa in a given assemblage. This is the level of diversity most commonly thought of when one considers of biodiversity. Taxonomic diversity is more precisely thought of as a measure of the richness and abundance of species. Generally, a large array of species types (high richness) coupled with relatively even proportions of each (relative abundance) is considered to have a high species diversity. Areas of high taxonomic biodiversity are typically given precedence in conservation efforts. 

Ecosystem diversity is a broad view of biodiversity. It considers entire communities of species along with the dynamic interactions with their environments (local geography, natural resources, etc...). Loss of genetic or species diversity will affect the performance of ecosystems. Scientists generally assess diversity at this level as *functional diversity*, or trait diversity. This concerns the range of things that organisms actually do in ecosystems. It is measured as the diversity of the values and range of species traits that are important to the structure and dynamic stability of the community. For instance, species distribution, growth forms, or resource-use strategy are several components that may typically be considered when assessing functional diversity. 

Additionally, we may also consider the *alpha* (within) and *beta* (between) diversities of an ecosystem. Phylogenetic, taxonomic, and functional diversity are typically measured as alpha components. Species beta diversity can be completely decomposed into *species nestedness* and *species turnover*. Nestedness is a measure of structure within an ecological system, usually applied to describe the distribution of species across locations, or the interactions between species. Turnover refers to the rate or magnitude of change in species composition along spatial or environmental gradients. There are a numbers of established metrics associated with each type of biodiversity, but the true biodiversity of a given system is the realization of a complex interaction of all of these. 

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
                                            
Each of these three diversity measures have quite different units and mathematical properties, so they cannot be directly compared with each other. Indeed, comparisons and ratios of the same index on two samples could be misleading, as both the Shannon and Simpson indices are non-linear. In the late 1990s, master formulas that generated each of these diversity indices began appearing from varying disciplines. A key breakthrough came from Ecologist Mark Hill, who realized that all these master formulas could be transformed so that they generate a family of measures with an easily interpretable metric. 

### Hill Numbers 

{::nomarkdown}
\begin{equation}
\textbf{Hill Numbers:}\\
\textit{When $q > 0$ and $q\neq 1$}\ \ \ \ \ \ \ \ {}^{q}D = \Bigg(\sum_{i=1}^S p_i^q\Bigg)^{1/(1-q)}\\
\text{} \\
\text{When q = 1}\ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ {}^{1}D = exp\Big(-\sum_{i=1}^Sp_i ln p_i\Big) 
\end{equation}
{:/}

**Hill Numbers** are in units of *effective number of species* and is a function of the parameter $q$. This parameter determines the sensitivity to species relative abundance. The traditional biodiversity metrics can be found with simple transformations and by setting $q$ with *Species Richness (q=0), Gini-Simpson (q=1)*, and *Shannon entropy (q=2)*. It was found that this measurement of diversity obey another principle key in Economics - the *replication principle*: if we pool $N$ equally large, equally diverse communities with NO shared species, the diversity of this pooled community should be $N$ times the diversity of a single community. This now solves the previous problems of ratios and comparisons with the Shannon entropy and Gini-Simpsons indices, and supports the rules of inference used by Biologists. In addition to species richness and relative abundance, Hill numbers have recently been generalized to incorporate phylogentic distances as well. Hill numbers, or the effective number of species best quantifies certain concepts of diversity, particularly species and taxonomic. Many of the R packages commonly used for diversity analysis make direct use of Hill numbers. 

## Assessing Biodiversity in R

To better understand how we can assess diversity in R, let's consider an example with artificial set of data pertaining to bat species in from ten sites in subtropical Africa. Sites 1-6 are geographically clustered near an inselberg structure, while sites 7-10 are distant, but close to each other. Our goal is to compare metrics of diversity between the two general transects - one NEAR and another DISTANT from the inselberg. At each site, bats were captured, tagged, and number of variables recorded. Here's a glimpse at some of the data...

![](/img/posts/batdata.png){: width="700" style="display:block; margin-left:auto; margin-right:auto"}

Here we can observe six of the 36 species used in this study, along with several variables pertaining to functional diversity, such as Foraging Mode, and Roosting. Don't worry about how to interpret any particular scores in this data set just yet. Instead, let's begin by examining Taxonomic diversity between the transects. 

### Alpha Taxonomic Diversity 

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

The nature of these curves tell us the ecoregion near the inselberg has greater potential for numerous individuals or the same or differing species. 

The iNEXT package also has convenient functions for tabular display of output information from the  iNEXT object. 

```{r}
inselberg.dist$AsyEst
```

![](/img/posts/esttable.png){: style="display:block; margin-left:auto; margin-right:auto"}

### Alpha Functional Diversity 

Functional diversity is based on the use of functional species traits which are defined as biological attributes that influence organismal performance. This aims at taking a functional approach to community ecology, *independent of taxonomy*. A step beyond species richness, functional diversity is a powerful tool to link community composition to ecosystem properties. Typically, we split functional diversity into three independent facets - Richness, Evenness, and Divergence, as proposed by Villéger, et. al. 

Functional richness can be thought of as the amount of niche space occupied by the species within a community. Functional evenness is considered a measure of the regularity of the distribution of functional richness within this niche space. Alternatively, functional divergence refers to the divergence of the distribution of functional richness in this common space. 

Here, our data contain many previously mentioned functional characteristics of our bat species, such as body weight, roosting strategy, and foraging range. The R package **fundiversity** is designed to ease the ease the computational burden of classical indices of functional diversity. Each function available in this package computes a single index with two main inputs - a species by traits matrix and a site by species matrix. 

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

We are now ready to use the fundiversity package. Let's first calculate the functional richness for each site. So we have

```{r}
#Species by Traits matrix
traits_bats <- matrix(c(data$Body.Mass,data$Roosting,...), byrow = F,
                      nrow = nrow(data)));
rownames(traits_bats) <- c(data$Species);
colnames(traits_bats) <- c("Body Mass", "Roosting",...);

Species by Site function 
SpSiteOccMat <- function(data,Site = NULL,Species = NULL){
  pop = c();
  rowlist = c(unique(data$Site));
  collist = c(unique(data$Species));
  i=1;
  while (i<=length(rowlist)) {
    dfuse = data %>% dplyr::filter(Site == rowlist[i]);
    j=1;
    while(j<=length(collist)){
      add = ifelse(collist[j] %in% c(dfuse$Species), 1, 0);
      pop = c(pop,add);
      j=j+1
    }
    i=i+1
  }
  OccMat = matrix(pop, nrow = length(rowlist),
                  ncol = length(collist),
                  byrow = TRUE);
  colnames(OccMat) = collist;
  rownames(OccMat) = rowlist
  return(as.data.frame(OccMat))
}

sites_bat <- SpSiteOccMat(data);

#Calculates functional richness 
dplyr::arrange(fd_fric(traits_bat, sites_bat),FRic)
```

<center>
<figure>
<img src = "/img/posts/fricpic.png" width = 150 >
</figure>
</center>

We clearly see differences in the magnitude of functional richness estimates. Unlike iNEXT with taxonomic diversity estimates, fundiversity does not have a built-in bootstrapping feature to make inference. So, we will construct our own bootstrapped distribution on each site and construct 95% confidence intervals for comparison. To construct this bootstrapped distribution, we will repeatedly sample from our data, *with replacement*, and probabilities proportional to the relative abundance of each species within the study. More refined methods of bootstrapping based on relative abundance from each site may improve methods. We briefly display the relative proportions. 

<center>
<figure>
<img src = "/img/posts/relabun.png" width = 350 >
</figure>
</center>
And develop the bootstrapped distribution, with B = 1000 samples each of size n =100. The following code generates a $36\times1000$ matrix. Each column represents a sample of our 36 species of bats. In each sample of $n=100$, if a bat species was selected more than once, it remained a '1' as a count matrix. From this counts matrix, we sampled species from our species by traits matrix, and used this calculate the new FRic metric. 
```{r}
3Establishing the multinomial sampling distribution
B = 1000;
n = 100;

boot.multi.dist <- rmultinom(B, n, prob = c(rel_abun$Rel_Abun))

i = 1;
while (i <= B) {
  j = 1;
  while (j <= 36) {
    boot.multi.dist[j,i] = ifelse(boot.multi.dist[j,i] >=1, 1, 0);
    j = j+1;
  }
  i = i+1
}

#Bootstrap disrtibution of functional richness

boot.est.dist <- data.frame();
i = 1;
while (i <= ncol(boot.multi.dist)) {
  boot.traits <- traits_bats[as.logical(boot.multi.dist[,i]),];
  boot.traits.scale <- scale(boot.traits, center = T, scale = T);
  boot.fun.rich.est <- fd_fric(boot.traits.scale, site_sp_bat);
  boot.est.dist <- rbind(boot.est.dist, boot.fun.rich.est);
  i = i+1
}

#Instead of typical boxplots, we will use 95% boxplots to make visual inference. 
quantiles_95 <- function(x) {
  r <- quantile(x, probs=c(0.01, 0.05, 0.5, 0.95, 1.0))
  names(r) <- c("ymin", "lower", "middle", "upper", "ymax")
  r
}
ggplot(data = boot.est.dist, aes(x = site,y = FRic, color = site)) + 
  stat_summary(fun.data = quantiles_95, geom = "boxplot") + 
  labs(title = "95% Percentile Bootstrap Functional Richness by Site") +
  xlab("Site") + ylab("Functional Richness")
```

<center>
<figure>
<img src = "/img/posts/funrichbox.png" width = 750>
</figure>
</center>
We observe that there is overlap in the 95% intervals, indicating there is no significant difference in functional richeness between sites. We can pool the data by transect and observe similar results.
<center>
<figure>
<img src = "/img/posts/funrichtrans.png" width = 750>
</figure>
</center>

Again, we observe overlap in the plots and cannot conclude any significant difference in functional richness between transects near the inselberg and distant from the inselberg. 

We follow a similar logic and present graphs for functional evenness and divergence. 

<center>
<figure>
<img src = "/img/posts/funevensite.png" width = 750>
</figure>
</center>

<center>
<figure>
<img src = "/img/posts/funeventrans.png" width = 750>
</figure>
</center>

<center>
<figure>
<img src = "/img/posts/fundivsite.png" width = 750>
</figure>
</center>

<center>
<figure>
<img src = "/img/posts/fundivtrans.png" width = 750>
</figure>
</center>

From these we observe no significant difference in functional divergence indices. Regardless, we have gained useful insight into the nature of these estimates at given sites and transects. 

### Beta Diversity

Beta diversity is generally thought of as the variation in species composition between sites. It can be the result of species replacement between sites (turnover) and species loss from site to site (nestedness). We can use the **betapart** package in R to compute total dissimilarity (Beta diversity) as Sørensen or Jaccard indices, as well as their respective turnover and nestedness components. With the *betasample()* function, we can establish a distribution of multi-site beta dissimilarity values from a randomized sample, and partition total dissimilarity into the independent turnover and nestedness components. The code and results for comparing the two inselberg transects are displayed below. 

```{r}
#Establish species by site matrices for each transect
beta.near <- SpSiteOccMat(dataNEAR);
beta.far <- SpSiteOccMat(dataFAR);

beta.near.samp <- beta.sample(beta.near.core,
                            sites = 6,
                            samples = 1000);
beta.far.samp <- beta.sample(beta.far.core,
                            sites = 4,
                            samples = 1000);

beta.dist.near <- beta.near.samp$sampled.values;
beta.dist.far <- beta.far.samp$sampled.values;

#Plot the beta dissimilariy and component sampling distributions
par(mar=c(5, 4, 5, 11), xpd=TRUE);

plot(density(dist.near$beta.SOR),
     xlim = c(0,0.8), ylim = c(0,75),
     xlab = 'Beta diversity', main = 'Figure 11: Beta Diversity within Transects', lwd = 1, col = "firebrick2");
lines(density(dist.near$beta.SNE), col = "firebrick2",lty=3,lwd=2);
lines(density(dist.near$beta.SIM), col = "firebrick2",lty=2, lwd=2);

lines(density(dist.far$beta.SOR), col = 'dodgerblue1', lwd=3);
lines(density(dist.far$beta.SNE), col = 'dodgerblue1', lty = 3, lwd=3);
lines(density(dist.far$beta.SIM), col = 'dodgerblue1', lty = 2, lwd =3);

legend("topright", inset=c(-.5, -.1), legend=c(expression(beta[ TOT]),expression(beta[ NEST]),expression(beta[ TURN])), lty=c(1,2,3), title="Beta Diversity");
legend("bottomright", inset=c(-.4, .1), legend=c("near", "far"),col = c("firebrick2","dodgerblue1"), pch=20)
```
<center>
<figure>
<img src = "/img/posts/betadiv.png">
</figure>
</center>

We can see that difference in total beta dissimilarity may be significant, as the distributions do not appear to overlap. It seems as though total beta diversity is higher in the sites near the inselberg, but we may want to implement a bit of integral analysis to determine overlapping areas and conclude for sure the level of significance. It appears to be the same case for turnover between the two transects. However, the distibutions of nestedness obviously overlap, so we can conclude there is no significant difference of nestedness among sites between the two transects. Finally, we can observe that the majority of beta dissimilarity between sites at each transect is explained by turnover, instead of nestedness. It is noteworthy that both transects display this pattern so similarly. 

### Alpha Phylogenetic Diversity 

I saved this diversity metric for the end, because we are going to step away from our bat example to demonstrate these methods. This is because metrics of this diversity primarily involve calculating phylogenetic distances of species. This distance is essentially the relative length on an evolutionary tree. Resources, such as GenBank and datasets in R, often provide the genetic information necessary to build these phylogeny trees. However, this data was not available yet for the species pertaining to our previous example. As of now, this is not particularly uncommon as the bank of ecophylogenetic information is still expanding. Instead, we will use a general dataset called "phylocom" provided in the R package **picante**. This package includes functions for analyzing the phylogenetic and trait diversity of ecological communities, comparative analyses, and the display and manipulation of phenotypic and phylogenetic data.

Phylocom contains samples of three objects types accepted by picante. The first, 'phylo', is phylogenetic data called a *phylo* object in R. Picante uses the *phylo* format implemented in the **ape** package in R to represent phylogenetic relationships among given taxa. Let's take a quick peak at the information contained in this type of data.

```
Phylogenetic tree with 32 tips and 31 internal nodes.

Tip labels:
  sp1, sp2, sp3, sp4, sp5, sp6, ...
Node labels:
  A, B, C, D, E, F, ...

Rooted; includes branch lengths.
```
From this, one can establish and plot a phylogenetic tree. Phylocom also contains community data, as formatted in the package **vegan**. Named 'sample', this is data matrix with sites/samples in the rows and taxa in the columns, like the aforementioned species by site presence/absence matrices. We must be sure that these column taxa names exactly match those of the species found in the corresponding phylogenetic data. Finally, our third element called 'traits' is traits data as we have already seen - taxa in rows and community traits in columns. Again, we wary of matching species names. If we choose, we can now visulize the provided data. 

We can visualize how taxa from six communities in this data set are arranged on the tree. First, we need to prune the phylogeny to only include the species at hand. Let's examine the 'sample' data dendrogram, where black dots represent species present in each of the six sommunty samples. 

```{r}
#Prune the tree
prunedphy <- prune.sample(comm, phy);

par(mfrow=c(2,3));

for (i in row.names(comm)) {
  plot(prunedphy, show.tip.label = FALSE, main = i)
  tiplabels(tip = which(prunedphy$tip.label %in% names(which(comm[i,] > 0))),
            pch = 19, cex = 2)
}
```
<center>
<figure>
<img src = "/img/posts/phyex1.png">
</figure>
</center>

Now, let's compute phylogenetic diversity with the Faith's (1992) PD index, which is defined as the total branch length spanned by the tree including all taxa in a local community. 

```{r}
pd(comm, phy, include.root = T)
```

<center>
<figure>
<img src = "/img/posts/PD.png" width = 750>
</figure>
</center>

The column 'PD' presents our Faith estimates, and 'SR' is the species richness. 

Alternatively, we can consider phylogenetic diversity as a form of species pairwise relatedness - take the average pair of individuals in a community and determine how genetically related they are. Metrics of community phylogenetic structure, such as mean pairwise distance (MPD), mean nearest taxon distance (MNTD), exist to analyze this type of phylogenetic diversity. Again, picante provides convenient functions to estimate these. Note the use of the 'cophenetic()' function, which converts a *phylo* object into a phylogenetic distance matrix. 

```{r}
phydist <- cophenetic(phy);

MPD <- ses.mpd(comm, phydist, null.model = "taxa.labels",
                abundance.weighted = F, runs = 99)
MPD
```

<center>
<figure>
<img src = "/img/posts/MPD.png" width = 750>
</figure>
</center>

Here, 'SES' refers to the standardized effect size is a measure of the differences of the observed genetic relatedness to a null model of phylogeny generated with some randomness. Several different null models exist to generate such measures. 

```{r}
MNTD < - ses.mntd(comm, phydist, null.model = "taxa.labels",
              abundance.weighted = F, runs = 99)
MNTD
```
<center>
<figure>

<img src = "/img/posts/mntd.png" width = 750>
</figure>
</center>

Clearly, previously used methods of bootstrapping can be used for comparison inference, but we will spare the reader from this monotony. 

## Conclusions

Hopefully this article has clarified what biodiversity actually means, how we may measure and assess it, and how R packages can greatly facilitate the analysis and discovery of biodiversity patterns from raw biological data. Biodiversity of ecoregions is a major factor taken into consideration when allocating resources for conservation efforts. Thus, it is crucial to have a solid understanding of diversity metrics and methods of analysis. Many of the metrics discussed in this article, such as Hill numbers, have only been developed in the past few decades. Hence, biodiversity assessment and analysis is an ever-evolving field as the underlying theories and mathematics graduate to incorporate more facets. Thank you so much for your time and please feel free to reach out to me! 