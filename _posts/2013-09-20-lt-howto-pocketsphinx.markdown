---
layout: post 
title: PocketSphinx Naudojamas
lang: lt
---


PocketSphinx
---------------------

Pocketsphinx yra biblioteka parašyta C kalba. Bibliotekos veikimas 
priklauso nuo kitos bibliotekos SphinxBase. Pastaroji turi 
suprogramuotą bendrą funkcionalumą daugumai CMU Sphinx projektų. 
Norint priversti veikti Pocketsphinx reikia turėti ir Sphinxbase.

PocketSphinx gali veiti MS Windows ir Linux OS.

Aplinkos ir bibliotekų pasiruošimas
----------------------

### MS Windows

Oficiliam puslapyje([Building application with pocketsphinx][url-pocketSphinx-build]) teigiama kad reikia turėti: 

*   MS Windows
*   MS Visual Studio 2008 ar naujesnės versijos

Sukompiliuoti išeities kodą:

*   Parsisiųsti naujausius  sphinxbase-X.Y ir pocketsphinx-X.Y paketus 
iš oficialaus puslapio: [Parsisiųsti][url-pocketSphinx-download]
*   Išpakuoti į tapačia direktorija tarkim c:\cmusphinx
*   Užkrauti sphinxbase.sln esantį c:\cmushinx\sphinxbase kataloge
*   Sukompiliuoti sphinxbase.sln kodą 
* 	Užkrauti pocketsphinx.sln esantį c:\cmushinx\pocketsphinx kataloge
*   Sukompiliuoti visą kodą

### Linux(Ubuntu)

Daugiau informacijos:

*   [Building application with pocketsphinx][url-pocketSphinx-build]
*   [Voice to text in Linux using PocketSphinx][url-PocketSphinx-linux]


### XCode instaliacija (iPhone)

Daugiau informacijos:

*   [Building application with pocketsphinx][url-pocketSphinx-build]

### Anroid instaliacija 

Daugiau informacijos:

* [Building Pocketsphinx On Android][url-pocketSphinx-android]

PocketSphinx Naudojimas
----------------------

### Programinės sąsajos dizaino pincipai

Biblioteka buvo kuriama atsižvelgiant į kuo lengvesnį panaudojimą 
programuotojui:

*   Naujos versijos turėtų lengviau užtikrinti istorinį 
suderinamumą dėl pakankamo abstrakčios sąsajos
*   Programos kodas leidžia turėtų keletą aktyvių atpažintuvų 
tame pačiame procese
*   Pakeitime sphinxbase leido ženkliai sumažinti programai 
reikalingos atminties.
*   PocketSphinx programuotojojo sąsaja gerai dokumentuota: [API 
aprašas][url-pocketSphinx-api]

### Programos kodo pavyzdys

Kompiliuoti Linux

```
gcc -o robotas_pc robotas_ps.c \
    -DMODELDIR=\"`pkg-config --variable=modeldir pocketsphinx`\" \
    `pkg-config --cflags --libs pocketsphinx sphinxbase`
```

Paleisti Linux

```
./robotas_pc
```

Kodas:

```
#include <pocketsphinx.h>

int
main(int argc, char *argv[])
{
	//Argumentai
	/////////////////////////////
	ps_decoder_t *ps;
	cmd_ln_t *config;
	FILE *fh;
	char const *hyp, *uttid;
        int16 buf[512];
	int rv;
	int32 score;

	//Inicializavimas
	/////////////////////////////
	config = cmd_ln_init(NULL, ps_args(), TRUE,
			     "-hmm",  "../models/hmm/lt.cd_cont_200/",
			     "-jsgf",  "../models/lm/robotas.gram",
			     "-dict",  "../models/dict/robotas.dict",
			     NULL);
	if (config == NULL){
		return 1;
	}
	ps = ps_init(config);
	if (ps == NULL){
		return 1;
	}

	//Failo atidarymas skaitymui
	/////////////////////////////
	fh = fopen("../test/audio/test1.wav", "rb");
	if (fh == NULL) {
		perror("Failed to open test1.wav");
		return 1;
	}

	//Visos bylos turinio dekodavimas
	/////////////////////////////
	rv = ps_decode_raw(ps, fh, "robotas", -1);
	if (rv < 0){
		return 1;
	}
	// Hipotezės ištraukimas
	/////////////////////////////
	hyp = ps_get_hyp(ps, &score, &uttid);
	if (hyp == NULL){
		return 1;
	}
	printf("Atpažinimo hipotezė: %s\n", hyp);

	// Bylos uždarymas
	/////////////////////////////
	fclose(fh);
    ps_free(ps);
	return 0;
}
```


[url-PocketSphinx-linux]: http://ghatage.com/2012/12/voice-to-text-in-linux-using-pocketsphinx/ 	"PocketSphinx In Linux"
[url-pocketSphinx-build]: http://cmusphinx.sourceforge.net/wiki/tutorialpocketsphinx	"Building application"
[url-pocketSphinx-android]: http://cmusphinx.sourceforge.net/2011/05/building-pocketsphinx-on-android/   "Building Pocketsphinx On Android"
[url-pocketSphinx-download]: http://cmusphinx.sourceforge.net/wiki/download "CMU Sphinx Downloads"
[url-pocketSphinx-api]: http://cmusphinx.sourceforge.net/api/pocketsphinx/  "api pocketsphinx"
