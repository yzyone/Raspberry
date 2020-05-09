FFMPEG Unknown input format 'alsa'

# Short answer #

    sudo apt-get install libasound2-dev

before attempting to configure and compile ffmpeg or anything where you need alsa support.

# Process for resolving similar build dependencies: #

Returning to this build/install method:

    $ cd /usr/src
    $ sudo git clone git://source.ffmpeg.org/ffmpeg.git
    $ cd ffmpeg/
    $ sudo ./configure && sudo make && sudo make install

the last line is a compaction of three steps, and the first one is significant as configure establishes what optional features the binary will support.

It is normal to change system libraries and configuration options and rerun configure many times until you are happy with its results.

The output you want is "alsa" as a supported indev (and probably outdev.)

```
./configure
  ... no alsa .. 
./configure --help
./configure --list-indevs
./configure --enable-indev=alsa --enable-outdev=alsa
  ... no alsa .. so it is not just off by default..
grep alsa configure
      alsa_asoundlib_h
      alsa
  alsa_indev_deps="alsa"
  alsa_outdev_deps="alsa"
  enabled_any alsa_indev alsa_outdev &&
      check_lib alsa alsa/asoundlib.h snd_pcm_htimestamp -lasound
```

this leads to why configure is not supporting alsa, and looking up asoundlib.h goes here though you could also query the packaging db.

    sudo apt-get install libasound2-dev
    ./configure
     ... alsa! ..
    ./make
    ./ffmpeg -formats |grep alsa
     ...  DE alsa! .. (the D/E columns indicate both directions are working)
    ./make install

---

https://raspberrypi.stackexchange.com/questions/70479/ffmpeg-unknown-input-format-alsa