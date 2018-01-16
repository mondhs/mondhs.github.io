---
layout: post
title: Senonai - kas tai
lang: lt
date: 2016-01-16 23:59
categories:
    - lt
summary: Informacija apie senonus
tags:
- ASR
- TTS
---


Senonai
---------------------

### CMU Sphinx Senone bendrai

Kaip rašo: (http://www.speech.cs.cmu.edu/sphinxman/opensource.html) tai yra bendras senone panaudojimo aprašymas:

Atpažįstant šneką reikia galimybės pridėti naujus žodžius. Kad nereiktų iš naujo mokyti šnekos modelio, kiekvieną kart priderant žodį, reikia kurti tinkamus šnekos modelius mokymo metu. Senonai yra šnekos modelio nepriklausomos sudėtinės dalys. Senonų sekos sudaro trifonų modelius. Kuris seka yra tinkama trifonui, yra sprendžiama naudojant "genėtą" sprendimų medį. Šio medžio kiekvieną lapą sudaro keletą fonemų(ir jų kontekstai?). CMU Sphinx kiekveina šaka(ir rinkinys) turi identifikacijos numerį, kurie vaidinami įrankyje:  "tied state id" arba "senone id". Tokia medžio šaka yra surišama su būsena arba senonu. 

Yra vienas "genėtas" sprendimų medis kiekvienos fonemos kiekviena būsenai(ne trifonio). fonema (kartu su visu kontekstu?) sudaro trifoną. Kuriant akustinį modelį, užtenka ieškoti sprendimo medžio lapo(kuris aprašo kontekstą) ir techniškai yra aprašomas kaip senone-id. Senone-id aprašo triphone būseną HMM modelyje. 


### Senone panaudojamumas(Nepabaigta)

Kitas puslapis: (http://www.speech.cs.cmu.edu/sphinxman/scriptman1.html#29)


Once the CD-untied (context dependent) models are computed, the next major step in training continuous models is decision tree building. Decision trees are used to decide which of the HMM states of all the triphones (seen and unseen) are similar to each other, so that data from all these states are collected together and used to train one global state, which is called a "senone". Many groups of similar states are formed, and the number of "senones" that are finally to be trained can be user defined. A senone is also called a tied-state and is obviously shared across the triphones which contributed to it. The technical details of decision tree building and state tying are explained in the technical section of this manual. It is sufficient to understand here that for state tying, we require to build decision trees.


### Kiek senonų man reikia?


(http://www.speech.cs.cmu.edu/sphinxman/fr3.html)


*   1-3h 	500-1000
*   4-6h	1000-2500
*   6-8h	2500-4000
*   8-10h	4000-5000
*   10-30h 	5000-5500
*   30-60h	5500-6000
*   60-100h 	6000-8000


### Kiek senonų naudota Liepa-1(Nebaigta)


TBP


### Dar vienas

(http://www.speech.cs.cmu.edu/sphinx/tutorial.html)

When multiple HMM states share the same Gaussian mixture, they are said to be shared or tied. These shared states are called tied states (also referred to as senones)


Each set of Gaussian mixtures is called a senone.HMMs can share senones


more Senones a model has, the more precise is the discrimination of the sounds and less is the recognition of unseen speech.

### Senonų įtaka atpažinimui(Nebaigta)

(https://www.sciencedirect.com/science/article/pii/S1110866516300123)

Rhe more Senones a model has, the more precise is the discrimination of the sounds and less is the recognition of unseen speech.
