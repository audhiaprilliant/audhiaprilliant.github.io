---
layout: post
title:  "Nazief and Adriani Algorithm - Stemming Algorithm for Indonesian Language"
author: audhi
categories: [ data mining, rprogramming ]
tags: [article]
image: assets/images/8-0.jpg
---
Text pre-processing is one of the vital and essential tasks in text mining and information retrieval. Text pre-processing is used to prepare unstructured data for knowledge extraction (Khedr et al. 2017). Several steps of text pre-processing, such as *tokenization, normalization,* and *stopwords removal.*

One of the text pre-processing is stemming. Stemming is one of two normalization. Stemming is the process of suffixes and/or affixes elimination for text searching, machine translation, document summarization, and text classification. For example in English document, the word **played** and **playing** will be transformed into the root word, **play**. Whereas for Indonesia, the stemming becomes so complicated due to the variety of Indonesian words that have prefixes, suffixes, infixes, and confixes.

In 2007, Mirna Adriani (University of Indonesia), Bobby Nazief (University of Indonesia), Jelita Asian (RMIT University), S.M.M Tahaghoghi (RMIT University), and Hugh E. Williams (Microsoft) introduced the Indonesia text stemming algorithm. The research entitled **"Stemming Indonesian: A Confix Stripping Approach"** was published in CM Transactions on Asian Language Information Processing. This algorithm is known as **Nazief and Adriani algorithm**.

For lemmatization in Bahasa Indonesia, it would be helped by Nazief and Adriani algorithm (2007) with the following rules:
1. Firstly, check terms that have not passed stemming. If terms are found, algorithm would be stopped
2. Inflection suffixes ("-lah", "-ku", "-nya", and "-mu") removal. But if there are several suffixes ("-tah", "-lah", "-pun", or "kah"), so the process would be looped to remove possesive pronouns ("-mu","-nya","-ku") in the dictionary
3. Derivation suffixes ("-kan", "i", "-kan") removal. But algorithm would be stopped if the term is found in dictionary. Otherwise, the following processes would be performed:
- If "-an" has been removed and at the end of word, there are letter "k", it would be removed. But if that term is found in the dictionary, algorithm stops. Otherwise, the process continues
- Suffixes from term that have passed removal stage ("an", "-kan", atau "-i") would be returned back
4. Derivational prefix ("be-", "me", "di-", "te-", "ke-", "pe-", or "se-") removal. If that term is found in the dictionary, algorithm stops. Otherwise, it needs recoding. If the output is mentioned in below description, algorithm would be stopped
- There is a combination like prohibited prefix and suffix
- The prefix is exactly similar with the others that have been removed
- There are three prefixes that are removed
5. If above steps have been passed, but the term is not found in dictionary, that term would be printed

For R language users, Nazief and Adriani algorithm can be applied using package *katadasaR* that created by Nurandi Setiabudi in 2015. Nurandi said that this package (katadasaR) contains several useful functions to remove:
- Prefixes, like *bertemu*, *dimakan*
- Suffixes, like *makanan*, *miliknya*
- Combination of prefixes and suffixes, like *pertemuan*,*mempermainkan*

For installation of katadasaR, user can easily use *devtools* function `devtools::install_github()` because of the package is in under development. The installation steps can use several commands:

```r
# install_packages('devtools')
library(devtools)
install_github('nurandi/katadasaR')
```

### Sources
<a target="_blank" href="http://www.mecs-press.org/ijisa/ijisa-v9-n7/IJISA-V9-N7-3.pdf" class="btn btn-danger">Khedr et al</a> <a target="_blank" href="https://www.researchgate.net/publication/220316701_Stemming_Indonesian_A_confix-stripping_approach" class="btn btn-warning">Research Gate</a> <a target="_blank" href="https://www.nurandi.id/blog/katadasar-stemming-bahasa-indonesia-dengan-r/" class="btn btn-primary">Nurandi Blog</a>
