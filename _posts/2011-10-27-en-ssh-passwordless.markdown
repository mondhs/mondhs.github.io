---
layout: post
title: Howto linux
lang: en
date: 2014-01-29 23:59
categories:
    - en
summary: howto linux
tags:
- ubuntu
- linux
- ssh
---

### How to setup passwordless SSH

More information in original [Original Post](http://everydaylht.com/howtos/system-administration/loggin-in-via-ssh-without-a-password/)

In order to use ssh without password with jenkins slave:

```
    $ssh-keygen -t rsa
    $ssh-copy-id -i ~/.ssh/id_rsa.pub MY_USER@MY_SERVER
    $ssh MY_USER@MY_SERVER
```

### How to mount a remote ssh filesystem using sshfs

[Original Post](http://embraceubuntu.com/2005/10/28/how-to-mount-a-remote-ssh-filesystem-using-sshfs/):

```
    sshfs example.com:/stuff /media/home-pc
```


### How to copy with Secure Copy (scp)

[Original Post](http://www.hypexr.org/linux_scp_help.php)
Copy the file "foobar.txt" from a remote host to the local host

```
    $ scp your_username@remotehost.edu:foobar.txt /some/local/directory 
```

Copy the file "foobar.txt" from the local host to a remote host

```
    $ scp foobar.txt your_username@remotehost.edu:/some/remote/directory 
```

### How to untar

```
    tar -zxvf file.tar.gz
```

### How to get durations

```
    find -iname '*.wav' -exec soxi -D '{}' \; | awk '{ SUM += $1} END { print SUM }'  
```



### How to run user application on Ubuntu startup 

This post should represent how it is possible on reboot start the application like tomcat.

More information Cron can be found: [Original Post](http://www.thegeekstuff.com/2009/06/15-practical-crontab-examples/)

It is possible define some time moments when some application should be executed. Also it can be used keywords like @reboot

First of all login to remote server as regular user(e.g. ssh MY_USER@MY_SERVER)

Access user lever cron configuration run in command line:

```
    $ crontab -e
```

text editor should show up. and you should add line in the end of the file with time and command like

```
    @reboot /home/MY_USER/tools/tomcat6nodes/tomcat_node6/bin/startup.sh
```

this will start tomcat shell command "startup.sh" after reboot.
