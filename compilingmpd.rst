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


Ok, let's install compiler tools

  ``sudo apt-get install build-essential automake autoconf libtool pkg-config libcurl4-openssl-dev 
  intltool libxml2-dev libgtk2.0-dev libnotify-dev libglib2.0-dev libevent-dev dh-make dev-scripts``

Now we can actually install MPD's dependencies


  ``sudo apt-get build-dep mpd``
  
Getting Sources
---------------

We could use GIT to clone the most recent MPD build. But, since we do care a lot about stability, and using Debianized source
will bring us the great advantage of dependencies listing, we'll download the source package from debian repos. 
First, lets create a new folder, to keep things clean and tidy.


  ``mkdir mpdcomp``
  
  ``cd mpdcomp``

Now let's download the sources with:

  ``wget http://ftp.de.debian.org/debian/pool/main/m/mpd/mpd_0.18.9.orig.tar.xz``
  
And unpack them with:

  ``tar xvf mpd_0.18.9.orig.tar.xz``
  
Move into sources folder: 

  ``cd mpd_0.18.9``
  
And Download the debian package source. This will allow us to build dependencies and other useful information into our .deb file

  ``wget http://ftp.de.debian.org/debian/pool/main/m/mpd/mpd_0.18.9-1.debian.tar.xz``
  
And unpack it

  ``tar xvf mpd_0.18.9-1.debian.tar.xz``

Done. All sources are ready. Let's move on.

Compiling
---------

As you may know, compiling gives us the ability to choose what features to included in our final package. Since we are using a debianized
source, we have an handy way to configure, instead of relying on .\configure:

We simply do that by edting the ``debian/rules`` file. So:

  ``sudo nano debian/rules``
  
This is the relevant part, mine looks like this (tailored to Volumio features)
  ``###############################################################
## Define Debian feature set (in configure summary order)

# Archive support
ENABLE_ARCHIVES=--disable-bzip2 \
	        --disable-iso9660 \
		--disable-zzip

# Autodiscovery support
ENABLE_AUTODISCOVERY=--with-zeroconf=avahi

# Client support
ENABLE_CLIENTS=--disable-ipv6 \
	       --enable-tcp \
	       --enable-un

# File format support
# Note: OggTremor / OggVorbis are in WITH_TREMOR
ENABLE_FILEFORMATS=--enable-aac \
		   --enable-adplug \
		   --enable-sidplay \
		   --enable-ffmpeg \
		   --enable-flac \
		   --disable-fluidsynth \
		   --disable-gme \
		   --enable-sndfile \
		   --enable-mikmod \
		   --enable-modplug \
		   --enable-mad \
		   --enable-mpg123 \
		   --enable-mpc \
		   --enable-opus \
		   --enable-audiofile \
		   --enable-wavpack \
		   --disable-wildmidi

# Other features
ENABLE_OTHER=--enable-lsr \
	     --enable-libmpdclient \
	     --enable-inotify \
	     --disable-sqlite

# Metadata support
ENABLE_METADATA=--enable-id3

# Playback support
# Note: ALSA is in WITH_SYSTEMD
ENABLE_PLAYBACKS=--enable-fifo \
		 --disable-recorder-output \
		 --enable-httpd-output \
		 --disable-jack \
		 --disable-ao \
		 --disable-oss \
		 --enable-openal \
		 --enable-pipe-output \
		 --enable-pulse \
		 --enable-roar \
		 --enable-shout``
		 
Save it. Ready to compile! 

Now, we can create a deb with 

  ``debuild binary``
  
Take a cup of coffee, tea. Whatever. This will last few minutes depending on your machine's horsepower.

Once it's done. In top level directory, you'll have:

  ``mpd-0.18.9                  mpd_0.18.9-1_armhf.deb
  mpd-dbg_0.18.9-1_armhf.deb  mpd_0.18.9.orig.tar.xz``
  
What we are looking for is ``mpd_0.18.9-1_armhf.deb`` which is our actual .deb installer. To install it:

  ``dpkg -i mpd_0.18.9-1_armhf.deb``

If we've done everything allright, we'll have MPD 18.9 up and running in few moments. To check the version and features:

  ``mpd --version``
  
Volumio's MPD
-------------

This is how Volumio's MPD looks like:

``Music Player Daemon 0.18.9

Copyright (C) 2003-2007 Warren Dukes <warren.dukes@gmail.com>
Copyright (C) 2008-2013 Max Kellermann <max@duempel.org>
This is free software; see the source for copying conditions.  There is NO
warranty; not even MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

Decoders plugins:
 [mad] mp3 mp2
 [mpg123] mp3
 [vorbis] ogg oga
 [oggflac] ogg oga
 [flac] flac
 [opus] opus ogg oga
 [sndfile] wav aiff aif au snd paf iff svx sf voc w64 pvf xi htk caf sd2
 [audiofile] wav au aiff aif
 [dsdiff] dff
 [dsf] dsf
 [faad] aac
 [mpcdec] mpc
 [wavpack] wv
 [modplug] 669 amf ams dbm dfm dsm far it med mdl mod mtm mt2 okt s3m stm ult um                                                                                        x xm
 [mikmod] amf dsm far gdm imf it med mod mtm s3m stm stx ult uni xm
 [sidplay] sid mus str prg P00
 [adplug] amd d00 hsc laa rad raw sa2
 [ffmpeg] 16sv 3g2 3gp 4xm 8svx aa3 aac ac3 afc aif aifc aiff al alaw amr anim a                                                                                        pc ape asf atrac au aud avi avm2 avs bap bfi c93 cak cin cmv cpk daud dct divx d                                                                                        ts dv dvd dxa eac3 film flac flc fli fll flx flv g726 gsm gxf iss m1v m2v m2t m2                                                                                        ts m4a m4b m4v mad mj2 mjpeg mjpg mka mkv mlp mm mmf mov mp+ mp1 mp2 mp3 mp4 mpc                                                                                         mpeg mpg mpga mpp mpu mve mvi mxf nc nsv nut nuv oga ogm ogv ogx oma ogg omg ps                                                                                        p pva qcp qt r3d ra ram rl2 rm rmvb roq rpl rvc shn smk snd sol son spx str swf                                                                                         tgi tgq tgv thp ts tsp tta xa xvid uv uv2 vb vid vob voc vp6 vmd wav webm wma wm                                                                                        v wsaud wsvga wv wve
 [pcm]

Output plugins:
 shout null fifo pipe alsa roar openal pulse httpd

Encoder plugins:
 null vorbis opus lame wave flac

Input plugins:
 file curl ffmpeg mms cdio_paranoia

Playlist plugins:
 extm3u m3u xspf pls asx rss soundcloud cue cue

Protocols:
 file:// http:// https:// mms:// mmsh:// mmst:// mmsu:// gopher:// rtp:// rtsp:/      ``
/ rtmp:// rtmpt:// rtmps:// cdda://``

And this, in more human-readable output, just before compiling (with configure routines)

``########### MPD CONFIGURATION ############

Archive support:
        (-bzip2) (-ISO9660) (-ZIP)
Autodiscovery support:
        (+Avahi) (-Bonjour)
Client support:
        (-IPv6) (+TCP) (+UNIX Domain Sockets)
File format support:
        (+AAC) (+AdPlug) (-C64 SID) (+FFMPEG) (+FLAC) (-FluidSynth) (-GME)
        (+libsndfile) (-MikMod) (+MODPLUG) (+MAD) (+MPG123) (+Musepack)
        (+Opus) (-OggTremor) (+OggVorbis) (+WAVE) (+WavPack) (-WildMidi)
Other features:
        (+libsamplerate) (+libmpdclient) (+inotify) (-SQLite)
Metadata support:
        (+ID3)
Playback support:
        (+ALSA) (+FIFO) (+File Recorder) (+HTTP Daemon) (-JACK)
        (-libao) (-OSS) (-OpenAL) (-OS X) (-Pipeline)
        (+PulseAudio) (+ROAR) (+SHOUTcast) (-Solaris) (-WinMM)
Streaming encoder support:
        (+FLAC) (+LAME) (+Ogg Vorbis) (+Opus) (-TwoLAME) (+WAVE)
Streaming support:
        (+CDIO_PARANOIA) (+CURL) (-Despotify) (+Soundcloud)
        (+MMS)

##########################################``

There have been reports of less size if doing 

  ``strip /usr/bin/mpd``

I cannot confirm this, neither better performances after stripping. 






  
