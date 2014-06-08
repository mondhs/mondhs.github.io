---
layout: post 
title: Sphinx as Engine for Voice Command Device  
lang: en
date: 2014-06-08 23:59
categories:
    - en
summary: Overview how I use sphinx for computer control 
tags:
- ASR
- PYTHON
---


# Intro

I wanted to review approaches how sphinx could be integrated to speech Voice Command Device(VCD) aka my computer. For investigation purposes I took simplified scenario: browser control by voice. I used Lithuanian acoustic model. I used python as scripting language due consistency with other technology stack that I am using in project. I use pocketsphinx for speech recognition. In this page I will describe two different integration: remote and local. Both approaches has different set of cons and pros.



# Demo 

Here is the demo on things what is explained in more details after it.

<iframe width="420" height="315" src="//www.youtube.com/embed/7l5YbDNbgiA" frameborder="0" allowfullscreen></iframe>

In the beginning of this demo is show reaction comparison between remote and local integration. Remote integration is working now manually, user has to click blue button. When he/she release it wav is sent to server and processed, reaction is shown as recognized value under blue button. Bellow browser is python screen that shows what is recognized and then send instruction to browser to react. 

# Sphinx Engine

I used PocketSphinx speech recognition library from previous [weekend projects]({% post_url 2014-03-08-en-bot-dialog-system %}). I used acoustic models an additional tools from my [github/mondhs](http://github.com/mondhs/lt-pocketsphinx-tutorial/tree/master/impl). For local and remote solution was used identical sphinx version build out of sphinx trunk. Grammar and dictionary used same build specially for this project:
1. [browser.dict](http://github.com/mondhs/lt-pocketsphinx-tutorial/blob/master/impl/models/dict/browser.dict)
2. [browser-min.gram](http://github.com/mondhs/lt-pocketsphinx-tutorial/blob/master/impl/models/lm/browser-min.gram)


# Remote Integration

Voice Command Device is controlled by commands that is sent from remote machine. Remote machine is processing audio stream and context to come up with reaction that VCD. I used available for me DEV server for speech processing. I ussed python [flup]   implementation as WSGI Server.

## Prepare enviroment

To bring up server I need setup my user to start working:

1. setup isolated virtual env: virtualenv "myenv"
2. activated virtual env: source "myenv"/bin/activate
3. installed flup: pip install flup
4. deactivate
5. creadted app dir in ~/public_html/startup
6. added .htaccess:

                Options +ExecCGI
                RewriteEngine On                
                RewriteBase ~/myuser/startup
                RewriteCond %{REQUEST_FILENAME} !-f
                RewriteRule ^(.*)$ main.fcgi/$1 [QSA,L]
                
7. added main.fcgi file with code

                #!/home/myuser/public_html/myenv/bin/python
                # -*- coding: utf-8 -*-
                
                import re
                
                                
                def index(environ, start_response):
                    """This function will be mounted on "/" and display a link
                    to the hello world page."""
                    start_response('200 OK', [('Content-Type', 'text/html')])
                    return ['''Sveiki.''']
                
                
                def not_found(environ, start_response):
                    """Called if no URL matches."""
                    start_response('404 NOT FOUND', [('Content-Type', 'text/plain')])
                    return ['Not Found']
                
                # map urls to functions
                urls = [
                    (r'^$', index),
                ]
                
                def application(environ, start_response):
                   """
                    The main WSGI application. Dispatch the current request to
                    the functions from above and store the regular expression
                    captures in the WSGI environment as  `myapp.url_args` so that
                    the functions from above can access the url placeholders.
                
                    If nothing matches call the `not_found` function.
                    """
                    path = environ.get('PATH_INFO', '').lstrip('/')
                    for regex, callback in urls:
                        match = re.search(regex, path)
                        if match is not None:
                            environ['myapp.url_args'] = match.groups()
                            return callback(environ, start_response)
                    return not_found(environ, start_response)
                
                
                
                # FastCGI link: Apache/mod_fcgid <-> Python (WSGI server)
                # (odd enough for the first time: FastCGI<->WSGI wrapper/gateway)
                if __name__ == '__main__':
                    from flup.server.fcgi import WSGIServer
                    WSGIServer(application).run()
                

Then is there and ready I could get page by url [https://myserver/~myuser/startup](https://myserver/~myuser/startup). 

## Integrate Sphinx

This was challenging part. It should work like this browser listening user speech, browser streams audio to server through REST services, server feeds stream to sphinx engine, server generates instruction for client machine based on recognized text and context, client machine executes instruction sent from server.

### Browser Recorder

I managed build html that makes browser record user speech: [ui.html](http://github.com/mondhs/lt-pocketsphinx-tutorial/blob/master/impl/demo-py/sphinx-web/ui.html). Many thanks to Chris Wilson and Matt Diamond for javascript projects [AudioContext-MonkeyPatch](http://github.com/cwilso/AudioContext-MonkeyPatch) and [Recorderjs](http://github.com/mattdiamond/Recorderjs) as I have no clue how [Web Audio API](http://www.html5rocks.com/en/tutorials/getusermedia/intro/) is working. Issue that I faced that I could not figured out how I could use streams with this, so I just recorded full audio (VAD is manual) and sent audio file to server. This is reaaly bad for system reaction time to the user, especially for long commands. I used jquery AJAX library to PUT audio file to server RecognitionManager#uploadForRecognition(soundBlob).

I trained Sphinx acoustic models using 16kHz and 16bit audio files. After some time I found that it is not possible chose this audio properties at least in [Chrome](http://code.google.com/p/chromium/issues/detail?id=73062). It is using only 44.1kHz and 16bit stereo(why?) audio stream. So there is js code in recorder.js that merge to channels, so I save my network channel by half. After investigation I found that [audiolib.js](http://www.html5audio.org/2012/07/audiolibjs-and-its-tutorials.html) could downsample, but I found to complicated use in current project phase. So I sent to server side 44.1kHz 16bit mono MS wav file. 1.5s wav file sizes: 44.1kHz = 147,5 kB, 16kHz = 53,5 kB. For my network and server configuration it took same time as audio signal to send to server process is and send it back. But from user experience it looks like I needed to wait twice as long. 

### Upload to Server

When I have audio file recorder I need process it in server side [main.fcgi](http://github.com/mondhs/lt-pocketsphinx-tutorial/blob/master/impl/demo-py/sphinx-web/main.fcgi). I need some time figure out how handle uploading file with WSGI, but finally I found example: [thejimmyg/upload.py](http://github.com/thejimmyg/wsgi-file-upload). I saved file in tmp dir that it is not good from security perspective. I put implementation to support only one user per server as any way this [code smells](http://en.wikipedia.org/wiki/Code_smell). Next step it is feed this received file to sphinx. 

### Upload to Server

File that is received should be preprocessed downsampled from 44.1kHz to 16kHz. I could not find python script that could do as everyone recomends to use [sox](http://sox.sourceforge.net/). Issue that on server I could not use `apt-get install sox` due security restrictions, I needed build sox binary on my local machine and upload static sox version to server home dir. 

I build sox with command: `./configure --prefix=/tmp/sox --disable-shared && make && make install`. I tested with command if it is not requires libs for sure: `/home/myuser/bin/sox in44kHz.wav -b16 out16kHz.wav rate 16000 dither`. It worked like a charm. After this step I was ready to use sphinx. 

I found another good opportunity for "step on a rake". I thought it would be good use [SWIG](http://www.swig.org/) generate python libraries in order to have full control on sphinx. But I did not wanted to use sudo any where so no installation of sphinx library to root dirs. I complied on local machine sphinxbase and pocketsphinx with `make && make install DESTDIR=/tmp/test`. I copied file to server and they where working. I tested by wrting python script and executing it in terminal. I found that if python is not installed to root dirs I need to setup LD_LIBRARY_PATH properly:

                '''
                run:
                 LD_LIBRARY_PATH=/home/myuser/pocketsphinx-bin/usr/local/lib python sphinx_test.py
                
                @author: Mindaugas Greibus
                '''
                import sys, os
                
                print (os.environ.get('LD_LIBRARY_PATH')) 
                
                sys.path.insert(0, '/home/myuser/pocketsphinx-bin/usr/local/lib/python2.7/dist-packages/sphinxbase')
                sys.path.insert(0, '/home/myuser/pocketsphinx-bin/usr/local/lib/python2.7/dist-packages/pocketsphinx')
                
                from pocketsphinx import Decoder
                
                #MODELDIR = "../models"
                MODELDIR = "/home/myuser/pocketsphinx-bin/impl/models"
                
                # Create a decoder with certain model
                config = Decoder.default_config()
                config.set_string('-hmm', os.path.join(MODELDIR, 'hmm/lt.cd_cont_200/'))
                config.set_string('-jsgf', os.path.join(MODELDIR, 'lm/robotas.gram'))
                config.set_string('-dict', os.path.join(MODELDIR, 'dict/robotas.dict'))
                decoder = Decoder(config)
                
                decoder.decode_raw(open(os.path.join(MODELDIR, '../test/audio/varyk_pirmyn-16k.wav'), 'rb'))
                
                # Retrieve hypothesis.
                hypothesis = decoder.hyp()
                if hypothesis is not None:
                    print ('Best hypothesis: ', hypothesis.best_score, hypothesis.hypstr)
                    print ('Best hypothesis segments: ', [seg.word for seg in decoder.seg()])

Next thing I tried to put same thing to fcgi, but I found that it is [impossible](http://stackoverflow.com/questions/6543847/setting-ld-library-path-from-inside-python) change LD_LIBRARY_PATH as script it is loaded in memory and changes environment variable does not make any effect. So I switched to binary execution of sphinx using `subprocess` python library. After this step all recognition modules are working.

Next thing what I needed it control instruction from server side to client side. I found too much security consideration so thought to try local VCD approach instead. 




# Local Integration

My main goal it was control browser by voice. For that I need 2 things local speech recognition module and instruction evaluator.

As recognition module I used gui sphinx wrapper that I had written previously as there where already output parsing functionality [min-browser.py](http://github.com/mondhs/lt-pocketsphinx-tutorial/blob/master/impl/demo-py/min-browser/min-browser.py). 

For instruction evaluator module I reviewed DBus, remote API options in Ubuntu. Nothing really universal I could find. I chose  Firefox plugin to use [pmorch/FF-Remote-Control](http://github.com/pmorch/FF-Remote-Control). This plugging allow execute js in the web page scope. I had to add other functions like new tab, as this is not possible do inside the page scope. Changed code could be found in [overlay.js](http://github.com/mondhs/FF-Remote-Control/commit/6d2f85860610dd2c4be7d71e5917acd50d04a7d3).

To use this approach I launch browser first, turn on the plugin(as it is off by default), run min-browser.py and Voila. 



[flup]: https://wiki.toolserver.org/view/Python_WSGI "yEd is a powerful desktop application that can be used to quickly and effectively generate high-quality diagrams"
[WSGI]: http://en.wikipedia.org/wiki/Web_Server_Gateway_Interface "The Web Server Gateway Interface is a specification for simple and universal interface between web servers and web applications or frameworks for the Python programming language"

