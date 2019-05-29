---
layout: post
title: Privatus šnekos asistentas
lang: lt
date: 2019-05-27 23:59
categories:
    - lt
summary: Savo privataus asistento kūrimas
tags:
- Speech assitant
---


---------------------
---------------------

## Privatūs asistentai

Jau yra keletas atviro kodo asistentų, kurių pagrindinis tikslas užŧikrinti privatumą:
* [mycroft.ai](https://mycroft.ai/) - aktyvi bendruomenė. naudoja pocketshpinx atpažinimui. Bet sintezei naudoja Google TTS. Dauguma įgūdžiai pritaikyti JAV ir anglų kalbai.
* [Jasper](https://jasperproject.github.io/) - projektas nebeaktyvus.
* [Kalliope](https://kalliope-project.github.io/)

Noras būtų pažiūrėŧi į galimybes paruošti assitentą "pasidaryk pats" stiliumi. Tikslai tokio asistento būtų privatumas ir galimybė pritaiktyi "mažoms" kalboms. Asistento privalumas būtų informacijos paruošimas vartotojo balsine forma. Informacijos gavyba įprastu atveju reikės interneto, o tai jau kelia sunkumus tikram privatumo reikalavimams.   Reikės spręsti sekančias problemas.

### Techninė ir programinė įranga

Balso atpažinimui, reikia techninės įrangos: garso įrašymo ir atkūrimo, bendros paskirties skaičiavimo modulis. Reikės programinės įrangos  šnekos atpažintuvo,  ketinimų atpažintu, informacijos gavybos modulio, sintezatoriaus. Privatus asistentas atpažinimą ir sintezę turėtų atlikti pats, tad reikės balanso tarp kokių gebėjimų asistentas turi ir kokių skaičiavimų pajėgumų reikia.

Techninė įranga
* Vienos plokštės kompiuteris - bendros paskirties skaičiavimo modulis: manau reiktų kad būtų bent 4 branduoliai.
    * Raspberry PI
    * Orange Pi
* Atkūrimo įranga - reikia mažo garsiakalbio, bet, kad gerai atkurtų žemus garsus. Tam gali padėti ir dizainas. Pridedant akustinius kanalus garsą galima padaryti "švaresnį"
* Įrašymo įranga - norint pagerinti balso atpažinimą, triukšmo šalinimą, galima papildomos informacijos apie garsą pridedant daugiau mikrofonų
    * du mikrofonai - turi palengvinti triukšmų šalinimą. vis vien sunku kalba - kalboje
        * [ReSpeaker 2-Mics Pi HAT](https://www.seeedstudio.com/ReSpeaker-2-Mics-Pi-HAT.html) >10€
    * 4 mikrofonai - galima nustatyti balso kryptį ir judėjimą:
        * [ReSpeaker 4-Mic Linear Array Kit for Raspberry Pi](https://www.seeedstudio.com/category/Development-Platforms-c-1002/single-board-computer-c-950/category/Speech-Recognition-c-44/ReSpeaker-4-Mic-Linear-Array-Kit-for-Raspberry-Pi.html) >30€
    * 6 mikrofonai
        * [ReSpeaker 6-Mic Circular Array Kit for Raspberry Pi](https://www.seeedstudio.com/category/Development-Platforms-c-1002/single-board-computer-c-950/category/Speech-Recognition-c-44/ReSpeaker-6-Mic-Circular-Array-Kit-for-Raspberry-Pi.html) >40€ - Tiek turi Amazon Alexa
    * 16 mikrofonų?: https://github.com/introlab/16SoundsUSB

Norint ištraukti informacija galima naudoti techninę įrangą: [ReSpeaker Core v2.0](https://www.seeedstudio.com/category/Development-Platforms-c-1002/single-board-computer-c-950/category/Speech-Recognition-c-44/ReSpeaker-Core-v2-0.html) 100€.

Galima bandyti naudoti biblioteką ODAS – [Artificial Audition](https://introlab.3it.usherbrooke.ca/sam/ca/description-du-projet/software/odas/) (Université de Sherbrooke)
https://youtu.be/n7y2rLAnd5I

### Garso įranga

Parinkti mažą garsekalbį ir pridėti garso pagerinimo detalių:
* https://www.youtube.com/watch?v=zAtMlKbaPRE
* https://www.youtube.com/watch?v=YfgnyIci5gE

### Dizainas

Problematika paruošti patrauklų, sąlyginai mažą, bet ir sutalpinančias visas aukščiau išvardintas detalias. Problema kaip uždengti akustiškai pralaidžiai mikrofonus
