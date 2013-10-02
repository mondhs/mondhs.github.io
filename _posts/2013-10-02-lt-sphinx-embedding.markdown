---
layout: post 
title: Sphinx - technologiniai deigimai
lang: lt
categories:
    - lt
summary: CMU Sphinx atpažintuvo integracijos į programinius sprendimus.
tags:
- Telefonas
- Naršyklė
- Serveris
- Pocketshinx
- Sphinx-4
---


Atpažinimo įrankis
---------------------



Šiuo metu dauguma komercinių produktų šnekai atpažinti yra pagrįsti paskirstytų(internetinių) sistemų principu. Savistoviai sprendimai yra naudojami sprendimuose kai nėra įmano pasiekti interneto ar atpažinimą atlikti siaurame kontekste.

![Sphinx Deployment][img-sphinx-deployment]


CMU Sphinx - Savistoviai sprendimai
---------------------

Tokie sprendimai nereikalauja interneto visą reikia informaciją atpažinimui yra platinama kartu(kalbos, etalonų modeliai ir algoritmas).

Šnekos atpažinimas šiuo metu yra aktualus:

* Mobiliuosiuose įrenginiuose
* Asmeniniuose kompiuteriuose
* Naršyklėse

### Mobilieji įrenginiai

Šnekos atpažinimas šiuo metu yra plačiai išvystytas: [Apple Siri][url-apple-siri], [Android šnekos palaikymas][url-android-speechapi], [Nuance Dragon][url-dragon-voxscis] šnekos technologių sprendimai.

Dauguma šių sprendimų reikalauja internetinio ryšio. Galima numanyti, kad didžioji atpažinimo dalis vyksta nutolusiuose serveriuose, o telefonas tarnauja tik šnekos priėmimui, požymių gavybai ir komandų vykdymui. Tokie sprendimai, kartais nėra patogūs nes gali būti užtikrintas internetinis ryšys. Kartai abejonių kelia ir privačių duomenų apsauga. Šis sprendimas yra nepakeičiamas kuomet reikia vykdyti informacijos paiešką internete. Plačiau apie tai `Šneka su naršykle` skyriuje

Mobiliųjų įrenginių plačiausia žinomos operacinės sistemos: Android ir iOS. Šios sistemoms galima sukurti programėles kurios naudoja pocketsphinx speneliai sukompiluotą biblioteką. Pvz androidui daugiau info [Building Pocketsphinx On Android][url-pocketSphinx-android], XCode instaliacija (iPhone): [Building application with pocketsphinx][url-pocketSphinx-build].




### Asmeniniuose kompiuteriuose

Priklausomai nuo programinės įrangos ir OS naudojimo galima naudoti tiek Sphinx-4, tiek PocketSphinx atpažintuvo versijas. Sphinx-4 leidžia lengviau rašyti programas [WORE][url-wikipedia-WORE] principu: vieną kartą parašius visose operacinėse sistemose veiks vienodai.  Sphinx-4 vienas iš kūrimo tikslų buvo programinis lankstumas ir su tai susietas tinkinimo sudėtingumas, tai lemia kad panaudojamas gali būti per sudėtingas tam tikriems atvejams (greitas demonstracija kaip )


### Šneka su naršykle

Funkcionalumas šneka su naršykle galima pasiekti kelias būdais:

* Kliento pusėje atpažinimas
	*	Javascript kodas vykdomas pačioje naršyklėje. Projektas [pocketsphinx.js][url-pocketshpinx-js]. Javascipt kodas užima 6mb
* Serverio pusėje. Nagrinėta plačiau sekančiam skyriuje.


CMU Sphinx - Internetai sprendimai
---------------------
Internetiniai sprendimai reikalauja kad kliento pusėje būtų gaunamas garso signalas ir atlikti pradiniai signalo apraudojimai, kurie sumažina tinklo apkrovas. Toliau šių signalų duomenys su papildoma kontekstine informacija  yra siunčiami tinklu į serverį. Jame yra vykdomi atpažinimo uždavinio sprendimas. Serveris siunčia atgal klientui duomenis su galimom atpažinimo hipotezėm. Klientas yra atsakingas atpažinimo įvertinti hipotezes su turimu papildomu kontekstu jei reikia. Taip pat klientas pasirinkęs hipotezę vykdo atitinkamus veiksmus.

Naršyklės serverio pusėje atpažinimas:
	* [Spantus Speech][url-spantusspeech-server]




[url-pocketSphinx-android]: http://cmusphinx.sourceforge.net/2011/05/building-pocketsphinx-on-android/   "Building Pocketsphinx On Android"
[url-pocketSphinx-build]: http://cmusphinx.sourceforge.net/wiki/tutorialpocketsphinx	"Building application"
[url-spantusspeech-server]: http://spantusspeech-2.spantus.cloudbees.net/ "Spantus speech server""
[url-wikipedia-WORE]: http://en.wikipedia.org/wiki/Write_once,_run_anywhere "Write once, run anywhere"
[url-apple-siri]: http://www.apple.com/ios/siri/ 
[url-android-speechapi]: http://android-developers.blogspot.com/2010/03/speech-input-api-for-android.html "Android Speech API"
[url-dragon-voxscis]: http://techcrunch.com/2013/06/24/like-google-voice-but-live-outside-the-us-then-try-voxscis-new-io6andoid-app/ "Like Google Voice But Live Outside The US? Then Try VoxSci’s New iO6/Android App"
[url-pocketshpinx-js]:https://github.com/syl22-00/pocketsphinx.js "Speech recognition in JavaScript"

[img-sphinx-deployment]: {{ site.url }}/images/lt-sphinx-embedding_sphinx-deployment.png "Sphinx-4"
