---
title       : Towards an Ontology for Ecosystem Services
subtitle    : Knowledge Organization in Ecology 
author      : Claas-Thido Pfaff 
job         : 23.01.2014
framework   : io2012   # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : [mathjax, bootstrap]       # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
github      :
  author    : cpfaff
  repo      : knowledgeorganization
---




## Background

* Ecology
  - Inherently cross-disciplinary
  - Collaborative, Data intensive
  - Global data networks
    
* Potentially allow
  - various sources (heterogen.)
  - syntheses
  - more complex questions
  - spaning wide scales (space/time)

---

## Background

* Problems
  - find relevant data (keyword)

<img src="assets/img/google_search_functional_diversity.png" style="width: 400px"/>
*  

  - lack of metadata (new tools/no benefit)
  - variability in terminology
  - e.g 163 definitions of stability
  - categorized to 70 concepts

> Grimm et al. 1996

---

## Background

* What do we need?
</br>
* Deep integration of metadata (data about data)
  - Widely accepted tools (R)
  - Make it unobtrusive
  - Derive automatically
 
* Standardizations
  - Thesauri/Ontologies
  - Provide a formal meachanism 
  - to define terms and relations
  - They can improve 
  - (exploration, interpretation, integration)

---

## What is an ontology?

* An example ontology fragment
  - Concepts in ellipses
  - Arrows show relations (plus restrictions)

<img src="assets/img/what_is_an_ontology.png" style="width: 800px"/>

> Advancing ecological research with ontologies (Madin et al. 2008)

---

## How can we use ontologies?

* An example application
  - starts with semantic annotation
  - improve dataset search (e.g wet weight)

<img src="assets/img/semantic_mapping.png" style="width: 800px"/>

> Advancing ecological research with ontologies (Madin et al. 2008) 

---

## How can we use ontologies?

* An example application
  - ontology mediated merge of datasets

<img src="assets/img/ontology_mediated_merge_datasets.png" style="width: 750px"/>

> Supporting ecology as data intensive science (Michener et al. 2008) 

---

## How can we use ontologies?

* An example application
  - ontology mediated merge of datasets

<img src="assets/img/ontology_with_datasets_overlay.png" style="width: 750px"/>

> Supporting ecology as data intensive science (Michener et al. 2008) 

---

## How does this look like?

* At rOpenSci (http://ropensci.org/)
  - metadata integration (R)
  - Ecological Metadata Language (Standard)

* What it can do by now (e.g.)
  - read and write metadata
  - assist metatada creation
  - publish to KNB, Figshare (data repositories)
 
 <img src="assets/img/rOpenSci_logo.png" style="width: 300px"/>

---

## How does this look like?


```r
dat = data.set(river = c("SAC",  "SAC",   "AM"),
               spp   = c("king",  "king", "ccho"),
               stg   = c("smolt", "parr", "smolt"),
               ct    = c(293,    410,    210),
               col.defs = c("River site used for collection",
                            "Species common name",
                            "Life Stage", 
                            "count of live fish in traps"),
               unit.defs = list(c(SAC = "The Sacramento River", 
                                  AM = "The American River"),
                                c(king = "King Salmon", 
                                  ccho = "Coho Salmon"),
                                c(parr = "third life stage", 
                                  smolt = "fourth life stage"),
                                "number"))
```


https://github.com/ropensci/EML

---

## How does this look like?


```r
data.frame(dat)
```

```
##   river  spp   stg  ct
## 1   SAC king smolt 293
## 2   SAC king  parr 410
## 3    AM ccho smolt 210
```



```r
attributes(dat)$col.defs
```

```
## [1] "River site used for collection" "Species common name"           
## [3] "Life Stage"                     "count of live fish in traps"
```


---

## How does this look like?


```r
eml_config(creator="Claas-Thido Pfaff <claas-thido.pfaff@uni-leipzig.de>")
eml_write(dat, file="mydataset.eml", title="My test dataset")
```

```
## [1] "mydataset.eml"
```


* Upper part:


```r
<creator>
  <individualName>
    <givenName>Claas-Thido</givenName>
      <surName>Pfaff</surName>
  </individualName>
  <electronicMailAddress>claas-thido.pfaff@uni-leipzig.de</electronicMailAddress>
</creator> ...
```


---

## How does this look like?


```r
</physical>
      <attributeList>
        <attribute>
          <attributeName>river</attributeName>
          <attributeDefinition>River site used for collection</attributeDefinition>
          <measurementScale>
            <nominal>
              <nonNumericDomain>
                <enumeratedDomain>
                  <codeDefinition>
                    <code>SAC</code>
                    <definition>The Sacramento River</definition>
                  </codeDefinition>
                  <codeDefinition>
                    <code>AM</code>
                    <definition>The American River</definition>
                  </codeDefinition> ...
```


---

## Where can I use this?

* You can automatically publish your data 
  - figshare (data repo)
  - KNB (data repo)


```r
eml_publish("mydataset.eml", 
            description="Example EML file from EML", 
            categories = "Ecology", 
            tags = "EML", 
            destination="figshare")

[1] 903758
```


* Enhaces your visiblity 
  - your data is citable (DOI), sharable 

---

## Where can I use this?

* Machine readable data (metadata)
  - Inclusion into workflows

<img src="assets/img/example_dataset_kepler_eml.png" style="width: 600px"/>

* Human readable as well!
  - tools: morpho

--- 

## But wait! Your said unobtrusive?

* integration of thesauri

* My thesaurus (tematres)
  - http://tematres.befdata.biow.uni-leipzig.de/vocab/index.php

* Based on keywords (Repos)
  - BEF-China
  - FUN-Div
  - Jena-Experiment
  - Biodiversity Exploratories

---

## But wait! Your said unobtrusive?

* I created the `rtematres` package (on CRAN)
  - exploit a vocabulary on tematres
  - Fetch information


```r
rtematres.api.do(task = "fetchVocabularyData")$count_terms
```

```
## [1] "959"
```


* Fetch definition of a term (define/metadata) -> integrate into EML package


```r
rtematres.api.define(term = "plant organ")$description
```

```
## [1] "plant organ - a functional and structural unit of a plant"
```


---

## Use to fuzzy improve a search

* explore the vocabulary (narrow)


```r
rtematres.api.do(task = "fetchDown", term = "plant organ")$term
```

```
## [1] "branch"        "flower"        "fruit"         "inflorescence"
## [5] "leaf"          "root"          "seed"          "stem"         
## [9] "twig"
```


* explore the vocabulary (broader) 


```r
rtematres.api.do(task = "fetchUp", term = "plant organ")$term
```

```
## [1] "entity"      "eukaryotes"  "plant"       "plant part"  "plant organ"
```


> Claas-Thido Pfaff, Karin Nadrowski, Anne Lang (rbefdata, in prep)

---

## Use to fuzzy improve a search

* With rtematres querry BEF-China database (rbefdata on CRAN)


```r
datasets = bef.portal.get.datasets.for_keyword("plant organ")
as.character(datasets$title)
```

```
## [1] "Biomass Allometry Equations of Pilot Experiment (SP7)"                                             
## [2] "Carbon (C) and Nitrogen (N) Concentration (Root, Stem, Twig, Leaf) of 8 target species in the CSPs"
## [3] "Traits of ferns and herb species occuring in the CSPs"
```



```r
narrower_terms = rtematres.api.do(task = "fetchDown", term = "plant organ")$term
datasets = bef.portal.get.datasets.for_keyword(narrower_terms)
dim(datasets)
```

```
## [1] 74  2
```


> Claas-Thido Pfaff, Karin Nadrowski, Anne Lang (rbefdata, in prep)

---

## Whats next? An ontology!

* I thought (bottom up)
  - based on the extracted keywords 
  - too much details (~1800 terms)
  
* Thus I switched (top down)
  - focus: Ecosystem goods and services
  - input: DGVMs? TEEB, MEA
  - extend: by concepts (thesaurus)
  - 1. goal: improve search (biodiv data)

---

## Whats next? An ontology!

* A first raw draft (shown partially, Protégé)
  - e.g. biomes, ecosystem states/processes
  - Local environmental factors
  - resources ..

<img src="assets/img/ontology_draft.png" style="width: 700px"/>

---

## Summary

* Metadata and Standardizations
  - crutial components
  - data driven science
  - Thesauri/Ontolgies
  - speak same language
  - reduce redundancy
  
* ontologies allow
  - smart features (search/integration)
  - sensible aggregate of ecol data 
  - transfer knowledge to decision makers

---

<div class = "flushcenter">
     <h1>Thanks for your attention!</h1>
     <h3>Questions?</h3>         
</div>
</br>

> ‘Without concepts it is impossible to work scientifically. 
> The price for this, however, is that the concepts determine the
> ways and methods in which we perceive nature. Critical examination
> of the concepts of their field is therefore part and parcel of 
> every scientist’s obligations.
>
> -- *Grimm, V. and Wissel, C. (1997)* --

