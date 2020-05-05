---
title: "Installing libav on Ubuntu 14.04 Trusty"
description: ""
category: 
tags: [linux]
---
{% include JB/setup %}

Requirement: Got a video that was encoded using HEVC, couldn't play it on my device so have to convert it to mpeg4.  
The tool ffmpeg was replaced in previous releases (Ubuntu 14.04 Trusty) by the avconv program from the libav project.

To install avconv you need to install the libav-tools package:
{% highlight shell %}
sudo apt-get install libav-tools
{% endhighlight %}
avconv is very similar to ffmpeg, but they also have some differences in syntax.

{% highlight shell %}
{% endhighlight %}

To see what codecs your tool supports use:
{% highlight shell %}
avprobe InputVideo.mkv
{% endhighlight %}
Result was the libav that comes with the repos does not support hevc. Tried installing the hevc decoder.

NOTE: The following does NOT work, it only installs the decoder it does not the necessary libraries:
{% highlight shell %}
Install x265 codec on Ubuntu 14.04
sudo apt-add-repository ppa:strukturag/libde265
sudo apt-get update
sudo apt-get install gstreamer1.0-libde265
This PPA contains Ubuntu packages of the libde265 library, plugins for
the GStreamer framework, VLC and required dependencies.
The GStreamer plugin source is available at
https://github.com/strukturag/gstreamer-libde265
The VLC plugin source is available at
https://github.com/strukturag/vlc-libde265
You can get sample Matroska streams from
http://www.libde265.org/downloads-videos/
http://www.divx.com/de/hevc
http://labs.divx.com/node/127909
 More info: https://launchpad.net/~strukturag/+archive/ubuntu/libde265
{% endhighlight %}
Run avprobe again no hevc support so this only install the decoder hevc still not found so have to build libav 

So we have to compile libav from source.
Download [libav](https://libav.org/releases/libav-11.6.tar.gz)
Install necessary dependencies:
{% highlight shell %}
sudo apt-get install yasm make git build-essential -y
sudo apt-get build-dep libav-tools
The following NEW packages will be installed:
  doxygen frei0r-plugins-dev libasound2-data libasound2-dev
  libavahi-client-dev libavahi-common-dev libbz2-dev libcaca-dev
  libcdio-cdda-dev libcdio-dev libcdio-paranoia-dev libdbus-1-dev libdc1394-22
  libdc1394-22-dev libdrm-dev libfreetype6-dev libgcrypt11-dev libgl1-mesa-dev
  libglib2.0-dev libglu1-mesa-dev libgnutls-dev libgnutls-openssl27
  libgnutlsxx27 libgpg-error-dev libgsm1-dev libjack-dev libjbig-dev
  libjpeg-dev libjpeg-turbo8-dev libjpeg8-dev liblzma-dev libmp3lame-dev
  libogg-dev libopencore-amrnb-dev libopencore-amrnb0 libopencore-amrwb-dev
  libopencore-amrwb0 libopenjpeg-dev libopus-dev libopus0 liborc-0.4-dev
  libp11-kit-dev libpthread-stubs0-dev libpulse-dev libraw1394-dev
  libschroedinger-dev libsdl1.2-dev libslang2-dev libspeex-dev libtasn1-6-dev
  libtext-unidecode-perl libtheora-dev libtiff5-dev libtiffxx5 libva-dev
  libva-drm1 libva-egl1 libva-glx1 libva-tpi1 libva-wayland1 libva-x11-1
  libvdpau-dev libvdpau1 libvo-aacenc-dev libvo-amrwbenc-dev libvorbis-dev
  libvpx-dev libx11-dev libx11-xcb-dev libx264-142 libx264-dev libxau-dev
  libxcb-dri2-0-dev libxcb-dri3-dev libxcb-glx0-dev libxcb-present-dev
  libxcb-randr0-dev libxcb-render0-dev libxcb-shape0-dev libxcb-sync-dev
  libxcb-xfixes0-dev libxcb1-dev libxdamage-dev libxdmcp-dev libxext-dev
  libxfixes-dev libxshmfence-dev libxv-dev libxvidcore-dev libxvmc-dev
  libxxf86vm-dev mesa-common-dev texi2html x11proto-core-dev
  x11proto-damage-dev x11proto-dri2-dev x11proto-fixes-dev x11proto-gl-dev
  x11proto-input-dev x11proto-kb-dev x11proto-video-dev x11proto-xext-dev
  x11proto-xf86vidmode-dev xorg-sgml-doctools xtrans-dev
The following packages will be upgraded:
  libasound2
{% endhighlight %}

Commands for building libav
{% highlight shell %}
./configure --enable-libx265 --enable-gpl
make
sudo make install
{% endhighlight %}

Problems met when buliding:
{% highlight shell %}
/home/source/libav-11.6$ ./configure --enable-libx265 --enable-gpl
ERROR: x265 not found
{% endhighlight %}

This means it still can not found x265 so we have to install x265.

Building x265
{% highlight shell %}
sudo apt-get install mercurial cmake cmake-curses-gui build-essential yasm
hg clone https://bitbucket.org/multicoreware/x265
cd x265/build/linux
./make-Makefiles.bash
cmake gui g generate and exit
make
/home/source/x265/build/linux$ sudo make install
[ 64%] Built target common
[ 87%] Built target encoder
[ 87%] Built target x265-shared
[100%] Built target cli
[100%] Built target x265-static
Install the project...
-- Install configuration: "Release"
-- Installing: /usr/local/lib/libx265.a
-- Installing: /usr/local/include/x265.h
-- Installing: /usr/local/include/x265_config.h
-- Installing: /usr/local/lib/libx265.so.82
-- Installing: /usr/local/lib/libx265.so
-- Installing: /usr/local/lib/pkgconfig/x265.pc
-- Installing: /usr/local/bin/x265
-- Removed runtime path from "/usr/local/bin/x265"
{% endhighlight %}

So libx265 is installed, before installing libav remember to remove the repos libav:
{% highlight shell %}
sudo apt-get remove libav-tools
sudo apt-get autoremove
The following packages will be REMOVED:
  libavcodec54 libavdevice53 libavfilter3 libavformat54 libavresample1
  libdc1394-22 libopus0 libx264-142
{% endhighlight %}

After building libav, Run avprobe to check everything is working
{% highlight shell %}
/usr/local/bin/avprobe
/usr/local/bin/avprobe: error while loading shared libraries: libx265.so.82: cannot open shared object file: No such file or directory
{% endhighlight %}

This means the libraries have been installed in the wrong location, copy them to /usr/lib:
{% highlight shell %}
ll /usr/local/lib/libx*
-rw-r--r-- 1 root root 3600730 Mar 19 17:12 /usr/local/lib/libx265.a
lrwxrwxrwx 1 root root      13 Mar 19 17:16 /usr/local/lib/libx265.so -> libx265.so.82
-rw-r--r-- 1 root root 3444683 Mar 19 17:12 /usr/local/lib/libx265.so.82
sudo cp /usr/local/lib/libx265* /usr/lib
{% endhighlight %}

Check that the codec is now available as decoder and encoder:
{% highlight shell %}
avconv -codecs|grep hevc
avconv version 11.6, Copyright (c) 2000-2014 the Libav developers
  built on Mar 19 2016 17:33:32 with gcc 4.8 (Ubuntu 4.8.2-19ubuntu1)
DEV.L. hevc                 HEVC (High Efficiency Video Coding) (encoders: libx265 )
{% endhighlight %}

If you specify 'aac' as the audio encoder you might encounter the problem:
{% highlight shell %}
encoder 'aac' is experimental and might produce bad results.
Add '-strict experimental' if you want to use it.
sudo avconv -i InputVideo.mkv -c:v mpeg4 -c:a libmp3lame -f mp4 OutputVideo.mp4
sudo avconv -i InputVideo.mkv -c:v mpeg4 -c:a copy -f mp4 OutputVideo.mp4
sudo avprobe InputVideo.mkv > /tmp/InputVideo.mkv.avprobe.txt 2>&1
{% endhighlight %}
Use 'copy' to just copy the audio stream it is much easier.

sudo avconv -i InputVideo.mkv -vcodec mpeg4 -f mp4 OutputVideo.mp4  
sudo avconv -i InputVideo.mkv -vcodec mpeg4 -f mp4 OutputVideo.mp4  

If you have a video that is encoded in avc:  
avprobe InputVideo.mkv  
...  
Format                                   : AVC  
Format/Info                              : Advanced Video Codec  
...  
Then you have to enable libx264.

avconv -codecs|grep h264  
avconv version 11.6, Copyright (c) 2000-2014 the Libav developers  
  built on Mar 19 2016 17:33:32 with gcc 4.8 (Ubuntu 4.8.2-19ubuntu1)  
D.V.LS h264                 H.264 / AVC / MPEG-4 AVC / MPEG-4 part 10  
Here it shows it only has decoder no encoder.

./configure --enable-libx264 --enable-gpl  
Once it is built and installed you can check for its availability  
avconv -codecs | grep h264  
DEV.LS h264                 H.264 / AVC / MPEG-4 AVC / MPEG-4 part 10 (encoders: libx264 )

If you want both hevc and avc for example: input codec is hevc, output format to be h264 then enable both  
{% highlight shell %}
sudo apt-get install x264
cd libav-11.6
./configure --enable-libx264 --enable-libx265 --enable-gpl --enable-static
{% endhighlight %}

Some basic commands for transcoding:
{% highlight shell %}
sudo avconv -i InputVideo.mkv -c:v h264 -c:a copy -f mkv OutputVideo.mkv
Requested output format 'mkv' is not a suitable output format
sudo avconv -i InputVideo.mkv -c:v h264 -c:a copy OutputVideo.mkv
Error while opening encoder for output stream #0:2 - maybe incorrect parameters such as bit_rate, rate, width or height
sudo avconv -i InputVideo.mkv -c:v h264 -c:a copy -f mp4 OutputVideo.mp4
{% endhighlight %}
My results show that h264 encoding is too slow stick with mpeg4.

When it is running the console will show:  
frame=36086 fps=  9 q=24.8 size=  280410kB time=1505.05 bitrate=1526.3kbits/s   
frame=36090 fps=  9 q=31.0 size=  280442kB time=1505.21 bitrate=1526.3kbits/s   
frame=36094 fps=  9 q=31.0 size=  280484kB time=1505.38 bitrate=1526.3kbits/s   
frame=36098 fps=  9 q=24.8 size=  280550kB time=1505.55 bitrate=1526.5kbits/s   
frame=36102 fps=  9 q=31.0 size=  280586kB time=1505.71 bitrate=1526.6kbits/s   
frame=36106 fps=  9 q=31.0 size=  280626kB time=1505.88 bitrate=1526.6kbits/s   

