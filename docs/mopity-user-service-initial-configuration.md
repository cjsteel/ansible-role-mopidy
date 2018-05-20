

## mopity-user-service-initial-configuration.md

## Resources

- [Installing Mopity](https://docs.mopidy.com/en/latest/installation/debian/)



```shell

       To start the server and change a config value directly on  the  command
       line, run:

          mopidy --option mpd/enabled=false

       The  --option  flag  may  be repeated multiple times to change multiple
       configs:

          mopidy -o mpd/enabled=false -o spotify/bitrate=320

       The mopidy config output shows the effect of the --option flags:

          mopidy -o mpd/enabled=false -o spotify/bitrate=320 config
mopidy -o media_dir=/var/lib/mopidy/media

```



## Mopidy Installation

### Add the archive’s GPG key:

```shell
wget -q -O - https://apt.mopidy.com/mopidy.gpg | sudo apt-key add -
```

### Add the APT repo to your package sources:

```shell
sudo wget -q -O /etc/apt/sources.list.d/mopidy.list https://apt.mopidy.com/stretch.list
```

### Install Mopidy and all dependencies:

```shell
sudo apt-get update
sudo apt-get install mopidy
# Install the Debian packaged spotify extension
sudo apt-get install mopidy-spotify
# Install the Debian packaged local extension sqlite back end
#sudo apt-get install mopidy-local-sqlite
# Install the Debian packaged youtube extension
#sudo apt-get install mopidy-youtube
```

### Install unrar 

unrar is required to unarchive some free music we will download from the internet and use for testing.

```shell
sudo apt install -y unrar
```

## Mopidy-Local extension

Comes with Mopidy and is the default backend for all URIs starting with `local:`. The out of the box configuration includes a default local library storage, `json`. which works fine but requires Mopidy to be restarted to show changes available from the `json`to a local collection after scanning local media.

and allows playing of local music is enabled by default. We are going to install the SQLight extension as well.

## HTTP CLients

### Mopidy-Moped

https://github.com/martijnboland/moped

```shell
sudo apt-get install python-pip
sudo pip install Mopidy-Moped
sudo pip install --upgrade pip
```

### Local Images

* https://github.com/mopidy/mopidy-local-images

```shell
sudo pip install Mopidy-Local-Images
```

### mopidy-local-sqlite

https://github.com/mopidy/mopidy-local-sqlite

apt

```shell
sudo apt install -y mopidy-local-sqlite
```

pip

```shell
sudo pip install Mopidy-Local-SQLite
```

Configure

```shell
mopidy -o local/library=sqlite
```

Confirm with

```shell
mopity config
```

output and settings we are interested in:

```sehll
[local]
library = sqlite
scan_flush_threshold = 100
```

re-scan your library to populate the database:

```shell
mopidy local scan
```

## Local Media Directory

### Setup the local media directory

* mount or create directory
* populate it with some free flac format media

```shell
mkdir -p "$HOME/Music/1966-11-19, Fillmore Auditorium, San Francisco CA, SBD (94106)"
cd "$HOME/Music/1966-11-19, Fillmore Auditorium, San Francisco CA, SBD (94106)"
wget http://www.ousterhout.net/lossless/gd/1966-11-19,%20Fillmore%20Auditorium,%20San%20Francisco%20CA,%20SBD%20%2894106%29.rar
```

* un archive our free music

```shell
unrar x 1966-11-19\,\ Fillmore\ Auditorium\,\ San\ Francisco\ CA\,\ SBD\ \(94106\).rar
# if you want to delete the rar package then...
rm *1966-11-19\,\ Fillmore\ Auditorium\,\ San\ Francisco\ CA\,\ SBD\ \(94106\).rar
ls -al
```

Ensure the Mopidy user service media directory is configured for our new local media:

Edit `nano ~/.config/user-dirs.dirs` and the option for your local media (Music) directory:

```shell
nano ~/.config/user-dirs.dirs
```

Default content example:

```shell
# This file is written by xdg-user-dirs-update
# If you want to change or add directories, just edit the line you're
# interested in. All local changes will be retained on the next run
# Format is XDG_xxx_DIR="$HOME/yyy", where yyy is a shell-escaped
# homedir-relative path, or XDG_xxx_DIR="/yyy", where /yyy is an
# absolute path. No other format is supported.
#
XDG_DESKTOP_DIR="$HOME/Desktop"
XDG_DOWNLOAD_DIR="$HOME/Downloads"
XDG_TEMPLATES_DIR="$HOME/Templates"
XDG_PUBLICSHARE_DIR="$HOME/Public"
XDG_DOCUMENTS_DIR="$HOME/Documents"
XDG_MUSIC_DIR="$HOME/Music"
XDG_PICTURES_DIR="$HOME/Pictures"
XDG_VIDEOS_DIR="$HOME/Videos"
```

The manual says the follwoign should work but it does not!

```shell
# this does not work
#mopidy -o local/media_dir=~/Music_new
```

#### The `mopidy Command`

Check our configuration file(s)

```shell
mopidy config
```

Command output example: [output/output-mopidyctl-config-run-001.md](output/output-mopidyctl-config-run-001.md)

#### Scanning the local media collection

Next we scan the local media 

```shell
mopidy local scan
```

### restart the opidy service

```shell
service mopidy status
service mopidy enable
service mopidy stop
service mopidy start
```

## Testing our configuration

### Test your audio

```shell
gst-launch-0.10 audiotestsrc ! autoaudiosink
```

You should get a sound if everything has installed correctly

### Test mopidy via mpc

[mpc](http://linux.die.net/man/1/mpc) is a command line [mpd](http://mpd.wikia.com/wiki/Music_Player_Daemon_Wiki) client that works with Mopidy

```shell
sudo apt install -y mpc
mopidy &
mpc add spotify:track:1QXzQKmQiDOzGHwSXVdHTp
mpc play
```



## Additional Debian packaged extensions

You can try Python extensions. Keep in mind that python packages do not automatically install dependencies.

### List available extensions via apt

```shell
apt-cache search mopidy
```

Output from Ubuntu 16.04:

```shell
mopidy - music server with support for MPD/HTTP clients
mopidy-alsamixer - Mopidy extension for extension for ALSA volume control
mopidy-beets - Mopidy extension for playing music from Beets' web plugin
mopidy-dirble - Mopidy extension for browsing Dirble's radio station directory
mopidy-dleyna - Mopidy extension for playing music from Digital Media Servers
mopidy-doc - music server with support for MPD/HTTP clients - documentation
mopidy-internetarchive - Mopidy extension for playing music from the Internet Archive
mopidy-local-sqlite - Mopidy extension for keeping your local library in SQLite
mopidy-mpris - Mopidy extension for controlling playback through MPRIS
mopidy-podcast - Mopidy extension for searching and browsing podcasts
mopidy-podcast-gpodder - Mopidy extension for searching and browsing gpodder.net podcasts
mopidy-podcast-itunes - Mopidy extension for searching and browsing iTunes podcasts
mopidy-scrobbler - Mopidy extension for scrobbling music to Last.fm
mopidy-somafm - Mopidy extension for playing music from SomaFM
mopidy-soundcloud - Mopidy extension for playing music from SoundCloud
mopidy-tunein - Mopidy extension for playing music from TuneIn
mopidy-youtube - Mopidy extension for playing music from YouTube
mopidy-spotify - Mopidy extension for playing music from Spotify
mopidy-spotify-tunigo - Mopidy extension for providing the browse feature of Spotify
```

#### Mopidy user

The Mopidy service runs as the **mopidy** user, which is automatically created when you install the Mopidy package. 

The mopidy user will need read access to any local music you want Mopidy to play.





## Configure PulseAudio

When using PulseAudio, you will typically have a PulseAudio server run by your main user. 

Since Mopidy is running as its own user, it can’t access this server directly. Running PulseAudio as a system-wide daemon is discouraged by upstream (see [here](http://www.freedesktop.org/wiki/Software/PulseAudio/Documentation/User/WhatIsWrongWithSystemWide/) for details). Rather you can configure PulseAudio and Mopidy so Mopidy sends the sound to the PulseAudio server already running as your main user.

### Configure PulseAudio

#### Allow PulseAudio to accept sound over TCP from localhost 

Open `/etc/pulse/default.pa` for editing and uncomment the line `load-module module-native-protocol-tcp auth-ip-acl=127.0.0.1`

```shell
nano /etc/pulse/default.pa
```

* Do two page downs and look for the sction starting with "### Network access (may be configured with paprefs, so leave this commented" .
* Uncomment  the line **load-module module-native-protocol-tcp auth-ip-acl=127.0.0.1**
* This is what things should look like when you are done:

```shell
### Network access (may be configured with paprefs, so leave this commented
### here if you plan to use paprefs)
#load-module module-esound-protocol-tcp
load-module module-native-protocol-tcp auth-ip-acl=127.0.0.1
#load-module module-zeroconf-publish
```

#### Configure Mopidy to use this PulseAudio server:

Using this does not work, why???:

```shell
sudo mopidyctl -o audio/output=pulsesink server=127.0.0.1
```

manually adding this to `/etc/mopidy/mopidy.conf does work

```
[audio]
output = pulsesink server=127.0.0.1
```

See below for the whole file

```shell
[core]
cache_dir = /var/cache/mopidy
config_dir = /etc/mopidy
data_dir = /var/lib/mopidy

[logging]
config_file = /etc/mopidy/logging.conf
debug_file = /var/log/mopidy/mopidy-debug.log

[audio]
output=pulsesink server=127.0.0.1

[local]
media_dir = /var/lib/mopidy/media

[m3u]
playlists_dir = /var/lib/mopidy/playlists
```



After this, restart both PulseAudio and Mopidy:

```
pulseaudio --kill
start-pulseaudio-x11
sudo systemctl restart mopidy
```

If you are not running any X server, run `pulseaudio --start` instead of `start-pulseaudio-x11`.

If you don’t want to hard code the output in your Mopidy config, you can instead of adding any config to Mopidy add this to `~mopidy/.pulse/client.conf`:

```shell
default-server=127.0.0.1
```

# Advanced audio setups

Mopidy has very few [audio configs](https://docs.mopidy.com/en/latest/config/#audio-config), but the ones we have are very powerful because they let you modify the GStreamer audio pipeline directly. Here we describe some use cases that can be solved with the audio configs and GStreamer.

## Custom audio sink

If you have successfully installed GStreamer, and then run the `gst-inspect-1.0` command, you should see a long listing of installed plugins, ending in a summary line:

```
$ gst-inspect-1.0
... long list of installed plugins ...
Total count: 233 plugins, 1339 features
```

Next, you should be able to produce a audible tone by running:

```
gst-launch-1.0 audiotestsrc ! audioresample ! autoaudiosink
```

If you cannot hear any sound when running this command, you won’t hear any sound from Mopidy either, as Mopidy by default uses GStreamer’s `autoaudiosink` to play audio. Thus, make this work before you file a bug against Mopidy.

If you for some reason want to use some other GStreamer audio sink than `autoaudiosink`, you can set the [`audio/output`](https://docs.mopidy.com/en/latest/config/#confval-audio/output) config value to a partial GStreamer pipeline description describing the GStreamer sink you want to use.

Example `mopidy.conf` for using OSS4:

```
[audio]
output = oss4sink
```

Again, this is the equivalent of the following `gst-launch-1.0` command, so make this work first:

```
gst-launch-1.0 audiotestsrc ! audioresample ! oss4sink
```

## Streaming through Icecast

If you want to play the audio on another computer than the one running Mopidy, you can stream the audio from Mopidy through an Icecast audio streaming server. Multiple media players can then be connected to the streaming server simultaneously. To use the Icecast output, do the following:

1. Install, configure and start the Icecast server. It can be found in the `icecast2` package in Debian/Ubuntu.

2. Set the [`audio/output`](https://docs.mopidy.com/en/latest/config/#confval-audio/output) config value to encode the output audio to MP3 (`lamemp3enc`) or Ogg Vorbis (`audioresample ! audioconvert ! vorbisenc ! oggmux`) and send it to Icecast (`shout2send`).

   You might also need to change the `shout2send` default settings, run `gst-inspect-1.0 shout2send` to see the available settings. Most likely you want to change `ip`, `username`, `password`, and `mount`.

   Example for MP3 streaming:

   ```
   [audio]
   output = lamemp3enc ! shout2send mount=mopidy ip=127.0.0.1 port=8000 password=hackme
   ```

   Example for Ogg Vorbis streaming:

   ```
   [audio]
   output = audioresample ! audioconvert ! vorbisenc ! oggmux ! shout2send mount=mopidy ip=127.0.0.1 port=8000 password=hackme
   ```

   Example for MP3 streaming and local audio (multiple outputs):

   ```
   [audio]
   output = tee name=t ! queue ! audioresample ! autoaudiosink t. ! queue ! lamemp3enc ! shout2send mount=mopidy ip=127.0.0.1 port=8000 password=hackme
   ```

Other advanced setups are also possible for outputs. Basically, anything you can use with the `gst-launch-1.0` command can be plugged into [`audio/output`](https://docs.mopidy.com/en/latest/config/#confval-audio/output).

### Known issues

- **Changing track:** As of Mopidy 1.2 we support gapless playback, and the stream does no longer end when changing from one track to another.
- **Previous/next:** The stream ends on previous and next. See [#1306](https://github.com/mopidy/mopidy/issues/1306) for details. This can be worked around using a fallback stream, as described below.
- **Pause:** Pausing playback stops the stream. This is probably not something we’re going to fix. This can be worked around using a fallback stream, as described below.
- **Metadata:** Track metadata is mostly missing from the stream. For Spotify, fixing [#1357](https://github.com/mopidy/mopidy/issues/1357) should help. The general issue for other extensions is [#866](https://github.com/mopidy/mopidy/issues/866).

### Fallback stream

By using a *fallback stream* playing silence, you can somewhat mitigate the known issues above.

Example Icecast configuration:

```
<mount>
  <mount-name>/mopidy</mount-name>
  <fallback-mount>/silence.mp3</fallback-mount>
  <fallback-override>1</fallback-override>
</mount>
```

You can easily find MP3 files with just silence by searching the web. The `silence.mp3` file needs to be placed in the directory defined by `<webroot>...</webroot>` in the Icecast configuration.

© Copyright 2009-2018, Stein Magnus Jodal and contributors. Revision `18b82848`. 