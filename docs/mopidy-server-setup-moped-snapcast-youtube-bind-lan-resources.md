

* https://wiki.ecohackerfarm.org/tutorials:it:soundsystem:server_setup#install_the_moped_web_interface_to_mopidy

# Server setup

 This HowTo is about setting up the audio server on Ubuntu and  RaspberryPi. The instructions are mainly for Ubuntu and notes for RPi  are added where applicable. 

## 1. Make sure the server runs Avahi

 Avahi advertises services over network and Snapcast needs it. Run this as a sudoer. 

```shell
ps aux | grep avahi | grep -v grep || sudo apt-get install avahi-daemon
```

## 2. Install Mopidy

 **\*RPi:** Mopidy on RPi needs manual install for the  latest version. Dependencies may also differ (eg. GStreamer 1.0 not  0.10). These should get installed automatically from the new Mopidy  repository added to the local list. Follow these instructions: https://docs.mopidy.com/en/latest/installation/debian/#debian-install.*  

```shell
sudo apt-get install mopidy
```

 You can also install `mopidy-doc` for local documentation. 

## 3. Configure Mopidy for HTTP access and Snapcast integration

 When Mopidy runs as a service in the default single-zone mode, the main config file is `/etc/mopidy/mopidy.conf`. To see how Mopidy sees its configuration, run: 

```shell
sudo mopidyctl config
```

 When writing configs, make sure the parameters go under right headings.  The headings are the square brackets. The basic config entries to be  added to `/etc/mopidy/mopidy.conf` are the following. 

Edit /etc/mopidy/mopidy.conf

```shell
cp /etc/mopidy/mopidy.conf /etc/mopidy/mopidy.conf_orig
nano /etc/mopidy/mopidy.conf
```

### a) Snapcast server integration

```shell
[audio]
output = audioresample ! audioconvert ! audio/x-raw,rate=48000,channels=2,format=S16LE ! wavenc ! filesink location=/tmp/snapfifo-livingroom
```

The output config details have to agree with Snapcast server's configuration. 

### b) HTTP access to the web control interface

 The default port is 6680 and it can stay this way. But the `hostname` by default set to `127.0.0.1` has to be changed in order be available to other machines. 

```shell
[http]
hostname = 0.0.0.0
```

### c) Configure local music files location

Set additional music file locations to the `media_dirs` parameter under the `[file]` heading. 

For example: 

```powershell
[file]
media_dirs = /home/mopidy/Music
show_dotfiles = false
excluded_file_extensions =
  .jpg
  .jpeg
  .rar
follow_symlinks = false
metadata_timeout = 1000
```
and playlist location

```shell
[m3u]
# playlists_dir = /var/lib/mopidy/playlists
playlists_dir = /home/mopidy/playlists
```

### D) Add some media

add some music

```shell
rmdir /home/mopidy
cp -R /home/vagrant/Music/ /home/mopidy/.
chown -R mopidy /home/mopidy
```
More on the `file` extension here: <https://docs.mopidy.com/en/latest/ext/file/>. 
Binding LAN resources is described in a later point. 

#### scan music

```shell
mopidyctl local scan
```

### Restart mopidy

```shell
sudo systemctl restart mopidy.service
```

And now you should be able to access a Mopidy webpage at `localhost:6680` from the server, or at `192.168.X.X:6680` or a similar LAN address. There should also be a fifo created in `/tmp` with the name `snapfifo-livingroom` if you followed the instructions literally. 

 To have Mopidy started as service by default at system startup, run this: 

```
sudo systemctl enable mopidy.service
```

 Otherwise start and stop as needed with either 

```shell
sudo systemctl start mopidy.service
```

 or  

```shell
sudo systemctl stop mopidy.service
```

 For more Mopidy configuration details see <https://docs.mopidy.com/en/latest/config/>. 

## 4. Install the Moped web interface to Mopidy

 This is for easy web control of what's broadcast by the Mopidy server.  To install it, you probably need the Python package manager `pip`. Make sure you have it on your machine: 

```shell
sudo apt-get install python-pip
```

 Now install Moped itself. 

```shell
sudo pip install Mopidy-Moped
```

 After a Mopidy restart, you should be able to access the Moped interface at `localhost:6680/moped` from the server and the same port and path with a proper ip from another LAN machine. 

 You can find Moped's page at <https://github.com/martijnboland/moped>.  

## 5. Install Snapcast server

 Snapcast reads the music from Mopidy using the pipe in `/tmp`. Then it broadcasts it over LAN to Snapcast clients. The service is advertised automatically by Avahi (see step 1). You need to: 

 a) Download it from <https://github.com/badaix/snapcast/releases/tag/v0.10.0>. Choose a server package with a `.deb` extension and (`amd64`  before it for a standard laptop or PC). If you're using the graphical  interfacal, you can download it and then run it double-clicking on the  downloaded file. Or using the command line, get the file location, and  then do something like: 

```
  cd &&
  wget https://github.com/badaix/snapcast/releases/download/v0.10.0/snapserver_0.10.0_amd64.deb &&
  sudo dpkg -i snapserver_0.10.0_amd64.deb 
```

## 6. Configure the Snapcast server

 The configuration file should at in `/etc/default/snapserver`. The `SNAPSERVER_OPTS`  parameter needs to be set to integrate the Snapcast server with Mopidy.  Either add the following line to the config file, or edit the line  containing the `SNAPSERVER_OPTS` parameter to look like this: 

```
  SNAPSERVER_OPTS="-d -s pipe:///tmp/snapfifo-livingroom?name=livingroom&sampleformat=48000:16:2&codec=flac"
```

 Now reload the service with 

```
  sudo systemctl restart snapserver.service
```

 Snapserver runs as a service and by default should be starting by system  bootup. To get or change this behaviour follow the corresponding  instructions described in step 3. 

## 7. Install the YouTube exetension for Mopidy

 This extension facilitates restreaming the audio from YouTube videos.  The package is present in Ubuntu repositories. To install it from the  command line, type 

```
  sudo apt-get install mopidy-youtube 
```

 For playing Youtube GStreamer plugins are needed, in particualar this one: 

```
  sudo apt-get install gstreamer1.0-plugins-bad
```

 Restart Mopidy. Now the searchbox on the Moped page should be able to  find YouTube videos. You can also paste the whole YouTube addresses as  they appear in the browser while watching videos. 

 If resolving of URIs stops working, always try to update the pafy library first: 

```
  pip install --upgrade pafy
```

 For YouTube stream reading problems, try installing the Python `youtube-dl` library using pip: 

```
  sudo pip install youtube-dl
```

## 8. Bind Mopidy with LAN resources

 These instructions are for binding LAN resources accessible through Samba. 

### a) Create a mount of Samba resources

 Scan local services. 

```
  smbtree -S
```

 Find out the server address. 

```
  avahi-browse -ar
```

 Create a mount point and mount the samba directories. For this the package `cifs-utils` is needed. To set up a mount at system startup, append the following line to `/etc/fstab`: 

```
  //192.168.1.32/media/mukke /mnt/music cifs credentials=/root/.smb_music_credentials,uid=root,gid=root,ro 0 0
```

 And create the credentials file in `/root` with the following lines: 

```
  .smb_music_credentials
  username=Anonymous
  password=''
```

 To get all `/etc/fstab` entries mounted now, run: 

```
  sudo mount -a
```

 More on the `[local]` extension: <https://docs.mopidy.com/en/latest/ext/local/>. 

### b) New media need to be scanned and added to the library Mopidy uses

 The scan has to be executed with the right configs. If Mopidy runs as a service, check `systemctl` with a running mopidy.service instance to see the right configs. Then perform the following (with the right configs): 

```
  sudo mopidy --config /usr/share/mopidy/conf.d:/etc/mopidy/mopidy.conf local scan
```

 The default library is a json file. Another option is a `sqlite` database. 

 With a json library, Mopidy service needs a restart to see the rescanned media correctly. 

 Consider adding a `scan` action to `/etc/init.d/mopidy`. 

​                                   