############
Compiling MPD
############


Volumio uses a custom compiled Version of MPD ( http://musicpd.org ). A lightweight, powerful and brilliant Music Player Daemon
(you don't say...).

Using a compiled version brings some advantages over Debian mantained packages:

- Better performances both in term of responsiveness, and in system load (especially useful in low power platforms like the PI)
- Avoidance of unpredicted bugs, which can affect user experience
- Custom patching, that allows to add extra functionalities (not used in Volumio)
- The fun of compiling something

Volumio (from 1.3beta) uses MPD 18.9 

Start 
-----

First, we make sure to satisfy MPD's dependencies. We'll be using jessie's packages, since wheezy's one are outdated.

We do that by editing ``/etc/apt/sources.list`` and adding jessie's repos.

  ``sudo nano /etc/apt/sources.list``
  
Our file will look like this:
  
  ``deb http://http.debian.net/debian jessie main non-free
  deb-src http://http.debian.net/debian jessie main non-free`` 


Save with ``ctrl+x`` and then update the apt cache

  ``sudo apt-get update``


Now we can actually install MPD's dependencies


  ``sudo apt-get build-dep mpd``
