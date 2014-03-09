---
layout: post 
title: Speech Bot System
lang: en
date: 2014-03-08 23:59
categories:
    - en
summary: How to create Speech Bot? diagrams, descriptions and possible sollution
tags:
- AI
- TTS
- ASR
---

Intro
---------------------

A dialog system or conversational agent (CA) is a computer system intended to converse with a human, with a coherent structure. Dialog systems have employed text, speech, graphics, haptics, gestures and other modes for communication on both the input and output channel [Wikipedia-Dialog system].

This page present attacks problem on how it could be possible create simple speech dialog system. It will be reviewed simplified dialog flow, high-level design of the system, and some source code samples with Python. 


Dialog System
---------------------

Dialog system architecture is chosen on experience and components currently available. It is chosen to have pure audio speech communication without any graphical face rendering. System assumes that there is already system that dialog system is an additional interface for user.

![img-bot-system]

Dialog system diagram is defined above. System should have dialog Bot Interface(computer microphone and speakers, [Skype] or [Voip]) that allows user interaction say and hear natural language words. Next speech signal from Bot Interface is processed by Bot Automatic Speech Recognition module(CMU [Sphinx], [Julius], [Other ASR]). ASR produce text that should be interpreted by Artificial Intelligence(AI) see: Dialog Manager section below. AI should understand what user says, track dialog context and provide responses to user. AI textural response transformation to speech happens in Text to Speech module([FreeTTS], [Mbrola], [eSpeak], [Festival]).


Dialog Manager
---------------------
A dialog manager is a component of a dialog system , responsible for the state and flow of the conversation [Wikipedia-Dialog manager]. Dialog manager has input as human utterance, state machine to track context and output as instruction to system or user. Dialog manager is separate complicated problem that description is out of scope for this page. You can check other systems: CMU [Ravenclaw Olympus], [Opendial]. 

![img-bot-sequence]

In diagram above described strategic flow-control what will be used in this page. Main idea allow user order some service by voice dialog. First system should identify person. For authentication it should be used some unique information from user it might be unique system number and some personal information. When person is identified he/she could request some service. System should process request and try to match in existing list of services. If need service found then system submit request.



Code Sample
---------------------
Python source code [sphinx-skype-bot]. Python was chosen due easy integration between all system components. In future this could be revisited to use java instead.

Sample system implementation was gathered from many resources: [Mindcollapse Sample], [skypeautoanswer] and others.

### Bot Interface

[`SkypeBot.py`](https://github.com/mondhs/Sphinx-skype-bot/blob/master/sphinx-skype-bot/SkypeBot.py) class contains skype integration module. This class functionality is based on [Skype4Py] library. It requires skype configuration to allow software control skype instance. This implementation supports only one conversation per one skype active instance. 

Input audio stream is redirected through socket to server port 8080(see Bot Automatic Speech Recognition). Main method for this topic is `ProcessCall(...)`. 

    def ProcessCall(self, Call):
        """ Process call and emulate conversation """
        #Send audio stream to 8080 port of localhost. 
        Call.OutputDevice(Skype4Py.callIoDeviceTypePort, "8080")
        # eternal cycle for monitoring AI work in background
        while True:
            time.sleep(.100)
            #if AI created this file thats mean conversation should be ended
            if os.path.exists(self.killFile):
                #release lock and finish conversation
                os.remove(self.killFile);
                break;
            # if AI TTS created this wav file thats mean we need playback to user
            if os.path.exists(self.lockFile):
                # set file as playback in skype
                Call.InputDevice(Skype4Py.callIoDeviceTypeFile, self.lockFile)
                # pause thread execution while bot is speaking
                while not Call.InputDevice(Skype4Py.callIoDeviceTypeFile) == None:
                    time.sleep(.100)
                #release lock
                os.remove(self.lockFile);
        # terminate speech recording
        Call.OutputDevice(Skype4Py.callIoDeviceTypePort, None)
        time.sleep(1)
        Call.Finish()


### Integration ASR, AI and TTS

[`SkypeBotAsr.py`](https://github.com/mondhs/Sphinx-skype-bot/blob/master/sphinx-skype-bot/SkypeBotAsr.py) class contains server socket handler functionality that redirects stream to sphinx wrapper. It is listening on 8080 port

    server = SocketServer.TCPServer(("localhost", "8080"), BotAsrServer)

Sphinx wrapper (see ASR Sphinx Wrapper) was used as ASR. For Artificial Intelligence was used Decision Tree or if-then rule base logic see Artificial Intelligence. Recognition engine is used Sphinx Wrapper(see ASR Sphinx Wrapper).

`SkypeBotAsr#handle(...)` has main integration logic:


    def handle(self):
        #Create Sphinx instance
        sphinxWrapper = SphinxWrapper()
        #Initialize Sphinx with "code" grammar
        sphinxWrapper.prepareDecoder("code")
        #Initialize Artificial Intelligence context
        aiContext = self.ai.createContext();
        # invoke AI engine for the first time: welcome
        self.said(aiContext, None);
        # start waiting for speech segment
        sphinxWrapper.startListening()
        while True:
            #read skype data from socket in chunks
            data = self.request.recv(self.CHUNK)
            # feed received data to sphinx decoder
            sphinxWrapper.process_raw(data)
            # speech segment endend
            if sphinxWrapper.isVoiceEnded():
                #speech -> silence transition
                sphinxWrapper.stopListening();
                hypothesis = sphinxWrapper.calculateHypothesis();
                if hypothesis is not None:
                    messageSaid = hypothesis.hypstr
                    # feed ASR text to AI
                    self.processAsrText(aiContext, messageSaid)
                    if aiContext.state in aiContext.GRAM:
                        # if state is bound to grammar update ASR
                        sphinxWrapper.updateGrammar(aiContext.GRAM[aiContext.state])
                sphinxWrapper.startListening()

`SkypeBotAsr#processAsrText(...)` has main integration logic:

    def processAsrText(self, aiContext, message):
        aiContext = self.ai.said(text, aiContext)
        self.geneareSpeechResponse(aiContext.response)
        if aiContext.state == aiContext.STATE_FINISH:
            with open(self.killFile, 'a') as the_file:
                the_file.write('Bye\n')
            return None
        if aiContext.interactiveStep is False :
            self.processAsrText(aiContext, message);
        return aiContext

### ASR Sphinx Wrapper
Pocketsphinx engine modified [feature-vad branch] was used instead of main source code branch (added possibility to change grammars easily see [mondhs/pocketsphinx]). 

[`SphinxWrapper.py`](https://github.com/mondhs/Sphinx-skype-bot/blob/master/sphinx-skype-bot/SphinxWrapper.py) is used wrapper to simplify some of pocketsphinx operations. For sphinx decoder manipulation is used methods: 

* `prepareDecoder(...)` - Entry point where sphinx decoder is initialized or grammar updated it is call first time `createConfig(...)` and other `updateGrammar(...)`
* `createConfig(...)` - Create configuration with acoustic model path, grammar and dictionary
* `updateGrammar(...)` - Update decoder language model from fsg file

For audio stream feeding is used `process_raw(...)` method. It also updates vad status: if voice found in signal. Before signal is fed to decoder, it should be isntructed that new utterance is expected. When Vad says that speech segment ended it should be called `stopListening(...)`, only then we could request hypothesis what was said. `calculateHypothesis(...)`.

Homebrewed Lithuanian acoustic model was used see [lt-pocketsphinx-tutorial]. As acoustic model did not provide sufficient result with statistical language model it was used grammars instead. First was created [JSGF] gram files. Then files was transformed to computational friendly format fsg:

* create human readable grammar for particular case [code.gram](https://github.com/mondhs/Sphinx-skype-bot/blob/master/resource/code.gram)
* execute `sphinx_jsgf2fsg -compile yes -jsgf code.gram > code.fsg`
* You will get [code.fsg](https://github.com/mondhs/Sphinx-skype-bot/blob/master/resource/code.fsg)

Dictionary was created with [scripts](https://github.com/mondhs/lt-pocketsphinx-tutorial/tree/master/impl/scripts) from all [JSGF] gram files.

### Artificial Intelligence

[`Artificialintelligence.py`](https://github.com/mondhs/Sphinx-skype-bot/blob/master/sphinx-skype-bot/Artificialintelligence.py) defines dialog management logic. DTO pattern is used to define dialog context `AiContext` class. `Artificialintelligence#said(...)` is service that takes context and what was `said` - string extracted from speech segment by ASR. This method updates #context `AiContext#state` and `AiContext#response`.


### Text-to-Speech

`SkypeBotAsr#geneareSpeechResponse(...)` has main integration logic:

    def geneareSpeechResponse(self, text):
        #Invoke command file text-to-speech to generate Lithuanian language response for skype
        generateCommand = ["espeak-skype-lt.sh", text]
        p = subprocess.Popen(generateCommand, stdout=subprocess.PIPE)
        #wait till it is generated
        generateFile = p.stdout.readline().rstrip()
        #wait till skype will announce the fail to user
        while os.path.exists(self.lockFile):
            time.sleep(.100)

where [`espeak-skype-lt.sh`](https://github.com/mondhs/Sphinx-skype-bot/blob/master/sphinx-skype-bot/espeak-skype-lt.sh) is command line tool, that generates with eSpeak TTS engine file in 22kHz wav file. then it is resampled to 16kHz with sox tool. and as [Skype Wav format] is not common there is additional transformation [`to_mswav.py`](https://github.com/mondhs/Sphinx-skype-bot/blob/master/sphinx-skype-bot/to_mswav.py) needed.


[img-bot-system]: {{ site.url }}/assets/images/en-bot-system-diagram.png "Bot Dialog system"
[img-bot-sequence]: {{ site.url }}/assets/images/en-bot-dialog-flow.png "Bot Dialog sequence"

[Wikipedia-Dialog system]: http://en.wikipedia.org/wiki/Dialog_system "Dialog system description"
[Wikipedia-Dialog manager]: http://en.wikipedia.org/wiki/Dialog_manager "Dialog manager description"
[Ravenclaw Olympus]: http://wiki.speech.cs.cmu.edu/olympus/index.php/Olympus "Olympus dialog systems"
[Opendial]: http://opendial.googlecode.com "Opendial dialog systems"
[Skype]: http://en.wikipedia.org/wiki/Skype "Skype @ wikipedia"
[Voip]: http://en.wikipedia.org/wiki/Voip "Voice over Internet Protocol"
[Sphinx]: http://cmusphinx.sourceforge.net/ "CMU Sphinx - Open Source Toolkit For Speech Recognition"
[Julius]: http://julius.sourceforge.jp/en_index.php?q=index-en.html "Open-Source Large Vocabulary CSR Engine Julius"
[Other ASR]: http://en.wikipedia.org/wiki/List_of_speech_recognition_software "List of speech recognition software"
[FreeTTS]: http://freetts.sourceforge.net/docs/index.php "FreeTTS is a speech synthesis system written entirely in the Java"
[Mbrola]: http://tcts.fpms.ac.be/synthesis/mbrola.html "Speech synthesizer based on the concatenation of diphones"
[eSpeak]: http://espeak.sourceforge.net/ "eSpeak is a compact open source software speech synthesizer for many languages, for Linux and Windows" 
[Festival]: http://www.cstr.ed.ac.uk/projects/festival/ "The Festival Speech Synthesis System"
[sphinx-skype-bot]: https://github.com/mondhs/Sphinx-skype-bot/tree/master/sphinx-skype-bot "Skype integration with sphinx"
[skypeautoanswer]: http://code.google.com/p/skypeautoanswer/
[Mindcollapse Sample]: https://code.google.com/p/mindcollapse-com-blog-source/source/browse/small_projects/SkypeBot/__init__.py/ "Source code example of Skype bot"
[Skype4Py]: https://github.com/awahlig/skype4py "Python library which allows you to control Skype client application"
[feature-vad branch]: http://svn.code.sf.net/p/cmusphinx/code/branches/feature-vad/pocketsphinx/ "pocket sphinx with voice activity detection"
[mondhs/pocketsphinx]: https://github.com/mondhs/pocketsphinx "added possibility to change grammars easily"
[lt-pocketsphinx-tutorial]: https://github.com/mondhs/lt-pocketsphinx-tutorial/ "Lithuanian acoustic model training"
[JSGF]: http://www.w3.org/TR/2000/NOTE-jsgf-20000605/ "JSpeech Grammar Format"
[Skype Wav format]: https://github.com/awahlig/skype4py/issues/15 "Skype uses wav format with 46 bytes header"
