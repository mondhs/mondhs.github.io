---
layout: post
title: Ubuntu 12.04 VNC with xfce
lang: en
date: 2012-08-14 23:59
categories:
    - en
summary: howto install vnc and xfce on ubuntu server
tags:
- ubuntu
- linux
- vnc
- xfce
---


# Install VNC and xfce

More information in [Original post](http://skerit.com/en/computer/english-vnc-x11-session-on-ubuntu-12-04-server-without-monitor-or-graphics-card/)

```
    sudo apt-get install vnc4server
    sudo apt-get install xfwm4 xfce4-session
```

# Configure VNC 

*   vi /etc/init/vnc-server.conf

```
    # vnc-server.conf

    start on runlevel [2345]
    stop on runlevel [016]

    post-start script
            su MY_USER -c '/usr/bin/vnc4server :2 -geometry 1024x768'
    end script

    post-stop script
            su MY_USER -c '/usr/bin/vnc4server -kill :2'
    end script

    #End of File
```

*   sudo start vnc-server

*   vi /home/MY_USER/.vnc/xstartup

```
    #!/bin/sh
    
    # Uncomment the following two lines for normal desktop:
    unset SESSION_MANAGER
    #exec /etc/X11/xinit/xinitrc
    #gnome-session --session=gnome-classic &
    
    [ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
    [ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
    xsetroot -solid grey
    vncconfig -iconic &
    x-terminal-emulator -geometry 80x24+10+10 -ls -title "$VNCDESKTOP Desktop" &
    x-window-manager &
    startxfce4 &
```

# VNC applet



Known issue that password should be exposed in html. If this is not done applet crashes with access denied exception.

*   Downlaod zip with jar: http://www.tightvnc.com/download.php
*   Extract jar from zip and copy /home/MY_USER/public_home/vnc
*   create /home/MY_USER/public_home/vnc/index.html


```
    <html>
            <title>TightVNC desktop</title>
            <body>
                    <applet archive="tightvnc-jviewer.jar"
                            code="com.glavsoft.viewer.Viewer"
                            width="1200" height="800">
                            <param name="Host" value="MY_SERVER" /> <!-- Host to connect. Default:  the host from which the applet was loaded. -->
                            <param name="Port" value="5902" /> <!-- Port number to connect. Default: 5900 -->
                            <param name="Password" value="CORRECT_PASSWORD_HAS_TO_BE_HERE" /> <!-- Password to the server (not recommended to use     this parameter here) -->
                            <param name="OpenNewWindow" value="false" /> <!-- yes/true or no/false. Default: yes/true -->
                            <param name="ShowControls" value="yes" /> <!-- yes/true or no/false. Default: yes/true -->
                            <param name="ViewOnly" value="no" /> <!-- yes/true or no/false. Default: no/false -->
                            <param name="AllowClipboardTransfer" value="yes" /> <!-- yes/true or no/false. Default: yes/true -->
    
                            <param name="ShareDesktop" value="no" /> <!-- yes/true or no/false. Default: yes/true -->
                            <param name="AllowCopyRect" value="yes" /> <!-- yes/true or no/false. Default: yes/true -->
                            <param name="Encoding" value="Tight" /> <!-- Possible values: "Tight", "Hextile", "ZRLE", and "Raw". Default: Tight -->
                            <param name="CompressionLevel" value="" /> <!-- 1-9 or empty. Empty means some default -->
                            <param name="JpegImageQuality" value="" /> <!-- 1-9 or empty. When empty no jpeg compression used -->
                            <param name="LocalPointer" value="On" /> <!-- Possible values: on/yes/true (draw pointer locally), off/no/false (let server draw pointer), hide). Default: "On"-->
                            <param name="ConvertToASCII" value="no" /> <!-- Whether to convert keyboard input to ASCII ignoring locale. Possible values: yes/true, no/false). Default: "No"-->
    
                            <param name="colorDepth" value="" /> <!-- Reserved for future. Possible values: 6, 8, 16, 24, 32 (equals to 24). Only   24/32 is supported now -->
                            <param name="ScalingFactor" value="100" /> <!-- Reserved for future. Always 100% now -->
                    </applet>
                    <br />
                    <a href="http://www.tightvnc.com/">TightVNC Web Site</a>
            </body>
    </html>
```
 

