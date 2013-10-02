---
layout: post 
title: Kas yra CMU Sphinx
lang: lt
date: 2013-09-19 23:59
categories:
    - lt
summary: Sphinx istorija, aprašomos modifikacijų panašumai ir skirtumai.
tags:
- Sphinx-1
- Sphinx-2
- Sphinx-3
- Sphinx-4    
---



CMU Sphinx
---------------------

![Sphinx Logo][img-sphinx-4-logo]


Carnegie Mellon ([wikipedia][url-sphinx-wikipedia]) Universitete kuria automatinė šnekos 
atpažinimo sistemų grupę:  kelios versijos atpažinimo ir akustinio 
modelio mokymo programinė įranga. Oficiali svetainė: [CMU Sphinx][url-cmusphinx]


CMU Sphinx Versijos
---------------------

### Sphinx v1

Pirmoji versija buvo kuriama apie 1989 [[1]](#Lee1989). Algoritmas 
rėmėsi: paslėptų Markovo akustinių modelių (HMMs) ir n-gram statistiniu kalbos modeliu. 

programavimo kalba: C

Eksperimentiniams duomenimis buvo nustatyta klaidų: ~9%

### Sphinx v2

Apie 2000 metus buvo tobulinama antroji versija [[2]](#Huang1993). 
Programinė realizacija buvo kuriama taip kad būtų galima atpažinti 
šneką realiu laiku. Pagrinde buvo realizuotos šnekos aptikimo, 
dalinės hipotezės teikimo, dinaminis kalbos modelio užkrovimo 
algoritmo dalys. Buvo naudojamas pusiau-tolydaus akustinis modelis.

programavimo kalba: C

Eksperimentiniams duomenimis buvo nustatyta klaidų: ~9%

### Sphinx v3

2001 Trečioji versija turėjo leisti didesnį programinį lankstumą 
ir aukštą atpažinimo lygį [[4]](#Varela2003). Greitaveika nebuvo 
vienas iš tikslų. Su šia versija buvo pradėtas vystyti SphinxTrain paketas, kuris 
leidžia sulyginai lengviau apmokyti atpažinime naudojamus akustinius 
modelius. buvo naudojamas tolydus HMM

programavimo kalba: C

Eksperimentiniams duomenimis buvo nustatyta klaidų: ~10%

### Sphinx v4

2004 programinės įrangos iškelti tikslai [[5]](#Lamere2003)[[6]](#Walker2004)): tinkinimo lankstumas, tinkinimas, 
draugiška vartotojo/programuotojo sąsaja. Realizuojamo algoritmo 
pagrindas buvo paimtas iš Sphinx v3 Pilnai perrašytas su Sun(Oracle) 
Java. Sukurta papildomų Vartotojo sąsajos įrankių palengvinančių darbą.

Programavimo Kalba: Sun(Oracle) Java

Eksperimentiniams duomenimis buvo nustatyta klaidų: ~8%


### PocketSphinx

Sphinx algortimas buvo pakeistas tam, kad tikslingiau išnaudotų 
mobilių įrenginių techninę įrangą [[7]](#Huggins2006): 
fiksuoto-kablelio aritmetika, Gausinių mišinių modelių skaičiavimas. 

programavimo kalba: C

Eksperimentiniams duomenimis buvo nustatyta klaidų: ~10%


Nuorodos
---------------------
1. <a id="Lee1989"></a> Lee Lee, Kai-Fu. Automatic Speech Recognition: The Development of the Sphinx Recognition System. Vol. 62. Springer, 1989.
2. <a id="Lee1990">???</a> Lee, K-F., H-W. Hon, and Raj Reddy. "An overview of the SPHINX speech recognition system." Acoustics, Speech and Signal Processing, IEEE Transactions on 38.1 (1990): 35-45.
3. <a id="Huang1993"></a> Huang, Xuedong, et al. "The SPHINX-II speech recognition system: an overview." Computer Speech & Language 7.2 (1993): 137-148.
4. <a id="Varela2003"></a>  Varela, Armando, Heriberto Cuayáhuitl, and Juan Arturo Nolazco-Flores. "Creating a Mexican Spanish version of the CMU Sphinx-III speech recognition system." Progress in Pattern Recognition, Speech and Image Analysis. Springer Berlin Heidelberg, 2003. 251-258.
5. <a id="Lamere2003"></a>  Lamere, Paul, et al. "Design of the CMU sphinx-4 decoder." INTERSPEECH. 2003.
6. <a id="Walker2004"></a>  Walker, Willie, et al. "Sphinx-4: A flexible open source framework for speech recognition." (2004).
7. <a id="Huggins2006"></a>  Huggins-Daines, David, et al. "Pocketsphinx: A free, real-time continuous speech recognition system for hand-held devices." Acoustics, Speech and Signal Processing, 2006. ICASSP 2006 Proceedings. 2006 IEEE International Conference on. Vol. 1. IEEE, 2006.

paslėptų Markovo akustinių modelių (HMMs) ir n-gram statistiniu kalbos modeliu. 


[url-cmusphinx]: http://cmusphinx.sourceforge.net/ 	"CMU Sphinx"
[url-sphinx-wikipedia]: http://en.wikipedia.org/wiki/CMU_Sphinx        "CMUSphinx-Wikipedia"

[img-sphinx-4-logo]: {{ site.url }}/images/lt-what-is-sphinx_sphinx-4-logo.gif "Sphinx-4"


