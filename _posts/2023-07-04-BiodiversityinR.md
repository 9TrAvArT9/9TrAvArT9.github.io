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

## Assessing Biodiversity in R

Let's consider a synthetic set of data pertaining to two ecoregions in subtropical Africa. 