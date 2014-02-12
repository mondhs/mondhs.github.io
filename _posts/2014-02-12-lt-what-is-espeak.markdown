---
layout: post 
title: Kas yra Espeak
lang: lt
date: 2014-02-12 23:59
categories:
    - lt
summary: Espeak istorija ir bendra informacija
tags:
- Espeak
- TTS
---



Espeak
---------------------

![Espeak Logo][img-espeak-logo]

[http://espeak.sourceforge.net/][url-espeak-home]


Šiame puslapuje yra pristatoma „eSpeak“ balso sintezavimo sistema, pagrinde dėmesio skiriama lietuvių kalbos palaikymui.

[Wikipedijoje Espeak][url-espeak-wikipedia] yra aprašoma kaip kompaktiška atvirojo kodo programinė įranga balso sintezavimui. Ji veikia ant MS Windows ir Linux operacinių sistemų. Šnekos sintezavimo variklis naudoja formančių sintezavimą ir palaiko daugelių kalbų. Projektai NVDA, Ubuntu ir OLPC, Google Translate naudojo ar naudoja eSpeak.

eSpeak projekto kilmė yra naudojimas Acorn RISC OS sintezatorius, kuris buvo pradėtas rašyti 1995 Jonathan Duddington. Perrašyta versija Linux OS pasirodė 2006, o 2007 sausį pasirodė Windows SAPI 5 sudrinama versija.

Sintezatorius palaiko 68 kalbas. Kokybė sintezuojamų kalbų yra skirtinga, kadangi sintezavimo staklėmis aprašyti naudojama grįžtamoji informaciją iš gimtosios kalbos kalbėtojų ne visada yra pakankama.


Sintezatorius susideda iš dviejų dalių: grafema fonema transformacija ir fonema garsas transformacija. fonema->garsas galima naudoti ir kitus variklius(MBROLA) ([P. Kasparaitis. Diphone databases for Lithuanian text-to-speech synthesis. Informatica. 2005][url-espeak-lt-mbrolla])

Sintezatorius palaikos standartus:
* [http://en.wikipedia.org/wiki/Speech_Synthesis_Markup_Language](SSML)
* [http://en.wikipedia.org/wiki/Speech_Application_Programming_Interface](Windows SAPI 5)

paslėptų Markovo akustinių modelių (HMMs) ir n-gram statistiniu kalbos modeliu. 

Espeak programavimo pavyzdžiai
---------------------

Daugiau informacijos galima rasti: [github.com/mondhs/espeak-sample][url-espeak-sample]. 

## Komandinė eilutė

Norint patikrinti ar espeak veikia lietuviškai galima paleisti komandą komandinėje eilutėje:

```
    $espeak  -v mb-lt1  "11 valandų 56 minutės" -x 
    # Turi būti matomas fonemos v;ien'uol;ika val'andu: p;'ENk;ez;d;eS;imtS;eS'I m;in'ut;ees
    # turi būti girdima lietuviškais skaitmenimis tariama šneka
```
Audio pavyzdys: [Wav MBROLA balsas][url-espeak-lt-mbrola-sample]

## C++ kalboje

[C++ pavizdys sampleSpeak.cpp][url-espeak-sample-cpp]

```
    int main(int argc, char* argv[] ){
        char text[30] = {"11 valandų 56 minutės"};//Tariamas tekstas
        espeak_Initialize(AUDIO_OUTPUT_PLAYBACK, 500, NULL, 0 );//inicializavimas
        const char *langNativeString = "lt"; //Lietuvių kalba
        espeak_VOICE voice;//balso paruošimas
        memset(&voice, 0, sizeof(espeak_VOICE)); // atminties rezervavimas balso duomenų struktūrai
        voice.languages = langNativeString;
        voice.name = "klatt";
        voice.variant = 2;
        voice.gender = 1;
        espeak_SetVoiceByProperties(&voice);//nustatome balsą
        Size = strlen(text)+1;
        espeak_Synth( text, Size, position, position_type, end_position, flags,unique_identifier, user_data );
        espeak_Synchronize( );
        return 0;
    }
```
Audio pavyzdys: [Wav klatt2 balsas][url-espeak-lt-klatt2-sample]


Espeak Versijos
---------------------

### Pagrindinis kodas

Dabartinė žinoma versija 1.48.02 [espeak.sourceforge.net/download.html][url-espeak-download]

### Lietuvių kalba

Lietuvių kalba espeak yra palaikoma nuo 2012 metų 1.46.20 versijos. Dabartinė žinoma versija [github.com/mondhs/espeak][url-espeak-lt].
Grafemų-fonemų transformacijų taisyklės aprašytos: [github.com/mondhs/.../dictsource/lt_list][url-espeak-lt-rules]

### Kiti eSpeak susieją projektai
* Android OS sumaniems telfonams: Lietuvių kalbai: [github.com/mondhs/.../android][url-espeak-lt-android]
* Mokama atviro kodo projekto [github.com/rhdunn/espeak][url-espeak-en] versija Android OS [play.google.com/.../espeak][url-espeak-en-android]
* Arabų kalbai [https://github.com/arabic-tools/ar-espeak][url-espeak-ar]
* Java script naudojamų naršyklėja [github.com/kripken/speak.js][url-espeak-js]
* viso githube yra dabar [73][url-espeak-at-github] projektai susieti su espeak 


Espeak pasiruošimas tobulinimui
---------------------
Detalesnė informacija pateikta [Prisijungimas prie tobulinimo][url-espeak-lt-participation]. Parsiųsti galima iš github kodo saugyklos  ```git clone https://github.com/mondhs/espeak```

### Duomenų redagavimas
Toliau pateiktas svarbiausių aspektų aprašas lietuvių kalba.
### Grafema->Phonemos transformacijos taisyklės 
Už jas atsako '''espeak-data/dictsource''' kataloge esantys failai.
#### lt_rules 
Čia talpinamos pagrindinės transformacijos taisyklės.
Jos aprašomos pagal tokią struktūrą: 

```
    [ženklai prieš )] raidė(s) [( ženklai po] raidės tarimas.
```

Pavyzdžiui:

```
        d (_         t      // [d] žodžio gale tariami dusliai: kad – kat
     _) ie (šm       jie    // „ie“ žodžio pradžioje prieš „šm“ tariama [jie]: iešmas [jiešmas]
        i (A         ;      // A didžioji raidė reiškia bet kokį balsį, 
                            // kabliataškis (;) – minkštumo ženklą.
        k (CL06      k;     // C didžioji raidė reiškia bet kokį priebalsį, 
                            // L su dviem skaitmenimis nurodo tam tikrą raidžių derinių grupę, 
                            // kuri aprašoma to failo viršuje
     @) ė (jL12_     ee=    // lygu (=) nurodo, kad raidė yra kirčiuota, 
                            // eta (@) – prieš raidę yra bent vienas skiemuo: 
                            // priesaga -ėjas: siuvėjas
```

#### lt_list 
Čia aprašomi išimtiniai žodžių tarimo atvėjai.
Pavyzdys:
 manęs     $2         // kirčiuojamas antras skiemuo

#### lt_listx 
```lt_listx``` failas papildo ```lt_list``` failą, skrita kalboms kurios turi per daug išimtinių atvėjų.
### Fonemos 
Už fonemas atsako ```espeak-data/phsource/``` kataloge esantys failai. 
#### ph_lithuanian 
Ši interpretacija naudojama, kai pasirenkamas ```lt``` balsas.
#### mbrola/lt1 ir mbrola/lt2 
```espeak-data/phsource/mbrola/``` kataloge esantys failai aprašo espeak ir mbrola fonemų sąsajas; būtent ```lt1``` ir ```lt2``` failai aprašo fonemų interpretavimą, kai naudojamas ```mb/mb-lt1``` arba ```mb/mb-lt2``` balsas.
## Fonemų ir žodyno kompiliavimas 
Jį reikia atlikti, jei atlikote pakeitimus ```espeak-data/dictsource/lt_rules``` (tarimo taisyklės), ```espeak-data/dictsource/lt_list``` (įvardžių, sutrumpinimų, skaičių ir t.t tarimas), ```espeak-data/dictsource/lt_listx``` ar ```espeak-data/phsource/ph_lithuanian```, reikia iš naujo sukompiliuoti ```espeak-data/``` ```espeak-data/lt_dict```. 
Tam ```espeakedit``` grafinėje programoje eikite per meniu ```Compile > phoneme data``` ir ```Compile > dictionary lt```

Jei keitėte ```espeak-data/phsource/mbrola/lt1``` arba ```espeak-data/phsource/mbrola/lt2```, tuomet prieš tai dar reikia ```espeakedit``` grafinėje programoje eiti per meniu ```Compile > Compile mbrola phonemes list```.









## Nuorodos
---------------------
* Diskusijų forumas apie espeak-lt: [http://www.ubuntu.lt/forum/viewtopic.php?f=3&t=7439](http://www.ubuntu.lt/forum/viewtopic.php?f=3&t=7439)
* Espeak openSUSE sistemoje: [http://opensuse.lt/index.php?option=com_content&view=article&id=115:espeak-lietuviko-teksto-skaitymas&catid=20:programos&Itemid=2](http://opensuse.lt/index.php?option=com_content&view=article&id=115:espeak-lietuviko-teksto-skaitymas&catid=20:programos&Itemid=2)
* Klausimas eSpeak forume: [https://sourceforge.net/projects/espeak/forums/forum/538922/topic/5417656](https://sourceforge.net/projects/espeak/forums/forum/538922/topic/5417656 )
* Fonetika: [http://ualgiman.dtiltas.lt/fonetika.html](http://ualgiman.dtiltas.lt/fonetika.html)
* Kirčiavimas: [http://ualgiman.dtiltas.lt/kirciavimas.html](http://ualgiman.dtiltas.lt/kirciavimas.html)
* Internetinis kirčiavimo įrankis: [http://donelaitis.vdu.lt/main.php?id=4&nr=9_1](http://donelaitis.vdu.lt/main.php?id=4&nr=9_1)
* Apie kirčiavimą vikipedijoje: [http://lt.wikipedia.org/wiki/Lietuvi%C5%B3_kalbos_kir%C4%8Diavimas ](http://lt.wikipedia.org/wiki/Lietuvi%C5%B3_kalbos_kir%C4%8Diavimas )



[img-espeak-logo]: http://espeak.sourceforge.net/images/lips.png "Espeak logo"
[url-espeak-home]: http://espeak.sourceforge.net/ "Espeak svetainė"
[url-espeak-wikipedia]: http://en.wikipedia.org/wiki/ESpeak	"Espeak wikipedia"
[url-espeak-lt]: https://github.com/mondhs/espeak "Espeak lietuvių kalba"
[url-espeak-lt-android]: https://github.com/mondhs/espeak/tree/master/android "Espeak Android OS lietuvių kalba"
[url-espeak-lt-participation]: [https://github.com/mondhs/espeak/wiki/Participation "Prisijungimas prie tobulinimo"
[url-espeak-lt-rules]: https://github.com/mondhs/espeak/blob/master/dictsource/lt_list "Espeak transformavimo taisyklės"
[url-espeak-lt-klatt2-sample]: https://raw2.github.com/mondhs/espeak-sample/master/output/talkingClockEspeak-klatt2.wav "klatt2 espeak"
[url-espeak-lt-mbrola-sample]: https://raw2.github.com/mondhs/espeak-sample/master/output/talkingClockEspeak.wav "mbrola espeak"
[url-espeak-download]: http://espeak.sourceforge.net/download.html "Parsisiųsti espeak"
[url-espeak-en-android]: https://play.google.com/store/apps/details?id=com.reecedunn.espeak "eSpeak Android anglų k."
[url-espeak-lt-mbrolla]: http://iospress.metapress.com/content/we09ck6eb31n2ekr/ "Diphone databases for Lithuanian text-to-speech synthesis"
[url-espeak-en]: https://github.com/rhdunn/espeak "Espeak anglų kalba"
[url-espeak-ar]: https://github.com/arabic-tools/ar-espeak "Espeak arabų kalba"
[url-espeak-js]: https://github.com/kripken/speak.js "Espeak javascript"
[url-espeak-at-github]: https://github.com/search?q=espeak&ref=searchresults&type=Repositories "Espeak githube"
[url-espeak-sample]: https://github.com/mondhs/espeak-sample "Espeak programiniai pavyzdžiai"
[url-espeak-sample-cpp]: https://github.com/mondhs/espeak-sample/blob/master/sampleSpeak.cpp "Espeak cpp"

