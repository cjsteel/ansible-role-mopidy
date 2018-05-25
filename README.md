ansible-role-mopidy
===================

Under development - not working - An Ansible role to install and configure a Mopidy based music system

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself or the role should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required.

Role Variables
--------------

```shell
# for system service use "mopidyctl", for user service use "mopidy"

mopidy_command: mopidyctl

# Debian/Ubuntu/Raspberry Pi

mopidy_apt_gpg_key_url : https://apt.mopidy.com/mopidy.gpg
mopidy_apt_sources_list: https://apt.mopidy.com/stretch.list

mopidy_packages :
  - mopidy
  - mopidy-docs

# Mopidy configs

mopidy_system_config_overide : /etc/mopidy/mopidy.conf

# file

mopidy_file_overides
  - media_dirs : /var/lib/mopidy/media

# http
mopidy_http_overides:
  - hostname = 0.0.0.0     # for ip6 '::'

# m3u
mopidy_m3u_overides:
  - playlists_dir     : /var/lib/mopidy/playlists

# snapcast

mopidy_snapcast_ver: '0.10.0'

mopidy_snapcast_server_config : /etc/default/snapserver
mopidy-snapcast_client_config : /etc/default/snapclient
 
mopidy_snapcast_server_port  : 1704 # music
mopidy_snapcast_remote__port : 1705 # remote control

# mpd 
mopidy_mpd_overides
  - hostname = 0.0.0.0     # for ip6 '::'
```

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
