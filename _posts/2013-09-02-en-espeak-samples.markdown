---
layout: post
title: Espeak usage samples
lang: en
date: 2013-09-02 23:59
categories:
    - en
summary: define invoke espeak from code
tags:
- espeak
- tts
- c
- py
---



# Espeak usage samples 

It is tested on Ubuntu 12.04.

Before test you need check out code and build from: <a href="https://github.com/mondhs/espeak.">https://github.com/mondhs/espeak</a>.

Sample uses header files from source dir(<ESPEAK_SRC_DIR>). Example where source should be located: /home/user/src/espeak/src


# Files

## sampleSpeak.cpp
* <a href="https://github.com/mondhs/espeak-sample/blob/master/sampleSpeak.cpp">sampleSpeak.cpp</a> - Shows how to write simple cpp code to call espeak.
* Orignally downloaded from <a href="http://djkaos.wordpress.com/2009/12/27/simple-espeak-program-speak_lib/">http://djkaos.wordpress.com/2009/12/27/simple-espeak-program-speak_lib/</a>
* Compile(be sure you replace  <ESPEAK_SRC_DIR>): 
 * g++ -g -I.  -I <ESPEAK_SRC_DIR> sampleSpeak.cpp -lportaudio -lespeak -o sampleSpeak
* Run: ./speak

## audacityLabelSpeak.cpp
* <a href="https://github.com/mondhs/espeak-sample/blob/master/audacityLabelSpeak.cpp">audacityLabelSpeak.cpp</a> Shows how to write callback function.
* It stores generated audio file to wav and phoneme transcriptions. 
* Compile(be sure you replace  <ESPEAK_SRC_DIR>): 
 * g++ -g -I. -I <ESPEAK_SRC_DIR> audacityLabelSpeak.cpp -lportaudio -lespeak -o audacityLabelSpeak
* Run: ./audacityLabelSpeak
* As result will be generated audio <a href="https://github.com/mondhs/espeak-sample/blob/master/output/audacityLabelSpeak.au">audacityLabelSpeak.au</a> and audacity segmentation file <a href="https://github.com/mondhs/espeak-sample/blob/master/output/audacityLabelSpeak.phn.txt">audacityLabelSpeak.phn.txt</a>

 
## talkingClockEspeak.py

* <a href="https://github.com/mondhs/espeak-sample/blob/master/talkingClockEspeak.py">talkingClockEspeak.py</a> shows how to call espeak from python
* Run: ./talkingClockEspeak.py

## Code Analysis

* Mbrola Events are triggered: 
 * speak_lib#espeak_Synth()
 * ->speak_lib#sync_espeak_Synth()
 * ->speak_lib#Synthesize(text)
 * ->synthesize#Generate(phoneme_list)
 * ->synth_mbrola#MbrolaGenerate(phoneme_list===plist)
 * ->synth_mbrola#MbrolaTranslate()
 * ->synthesize#DoPhonemeMarker()
* Mbrola phoneme generation:
 * speak_lib#Synthesize(text)
 * ->synthesize#SpeakNextClause(text===p_text)(What is this: phoneme_callback)
 * ->TranslateClause(p_text)
 * ... ->translate#MakePhonemeList(){phlist = phoneme_list}
* tranlator created in translate#LoadVoices()

