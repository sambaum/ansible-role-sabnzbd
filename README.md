sabnzbd
=======

An Ansible role to setup and configure Sabnzbd on Ubuntu.

Requirements
------------

This role requires Ansible 2.0 or higher. Platform requirements are listed in the metadata file.
Make sure to download roles specified in **Dependencies** section if role installed **not** with Ansible Galaxy.

Overview
--------

List of tasks that will be performed under `sabnzbd` role:

1. Install and Configure Sabnzbd Daemon
2. Create Sabnzbd Complete and Incomplete Downloads folders
3. If `newsgroups_servers` is defined, configure usenet server and credentials
4. Configure Categories and use `nzbtomedia` postprocessing scripts

This role is dependant on `htpc-common` role which performs the following tasks:

1. Install ssh server to allow remote management.
2. Configure Zerconf networking and avahi-alias service.
3. Create htpc_user if user doesn't exist.
4. Enable sudo access for htpc user.
5. Create htpc media and downloads folders.

Downloads and Media folders layout if used with default variable values:

```
/mnt/media/
├── downloads               
│   ├── complete        # Complete Downloads
│   └── incomplete
│       ├── sabnzbd     # Sabnzbd Incomplete Downloads
│       └── process     # nzbtomedia processing folders
│           ├── movie
│           └── tv
├── movies
├── music
├── pictures
└── tv
```

Role Variables
--------------

```
# defaults file for sabnzbd

# Helper vaiable. In use by other roles
sabnzbd_enabled: yes

# Sabnzbd API Key
sab_apikey: c48afc846972e295826bb05d2e84dd59

# Sabnzbd Incomplete download locations
sabnzbd_incomplete: "{{ htpc_downloads_incomplete }}/sabnzbd"

# Helper vaiable. In use by other roles
sabnzbd_host: "{{ ansible_default_ipv4.address }}"

# Sabnzbd port. Default port 8080 conflicts with Kodi default port
sabnzbd_port: 9000
```

Optionally preconfigure usenet credentials ( See examples )

```
newsgroups_servers:
  - name:
    username:
    password:
    connections:
```

Dependencies
------------

Role Name | Description
----------|-----------
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.htpc--common-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/htpc-common/)| Create htpc user and media folders
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.nzbtomedia-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/nzbtomedia)    | Install NZBtoMedia Postprocessing

Variables defined in `GR360RY.htpc-common` role:

```
# defaults file for htpc-common

htpc_user_username: htpc
htpc_user_password: htpc
htpc_user_group: htpc
htpc_user_shell: /bin/bash
htpc_user_sudo_access: yes
htpc_ssh_service: yes
htpc_create_media_folders: yes
htpc_zeroconf: yes
htpc_media_path: /mnt/media
htpc_media_movies: movies
htpc_media_tv: tv
htpc_media_music: music
htpc_media_pictures: pictures
htpc_downloads_complete: "{{ htpc_media_path }}/downloads/complete"
htpc_downloads_incomplete: "{{ htpc_media_path }}/downloads/incomplete"
```

Variables defined in `GR360RY.nzbtomedia` role:

```
---
# defaults file for nzbtomedia

nzbtomedia_enabled: yes
nzbtomedia_path: /opt/nzbtomedia
```

Example Playbook
----------------

Install sabnzbd. Usenet credentials can be supplied through web interface.

```
- hosts: htpc-server
  become: yes
    
  roles:
    - role: GR360RY.sabnzbd
```

Install sabnzbd. Configure usenet server credentials.

```
- hosts: htpc-server
  become: yes

  vars:

    # Single or multiple news servers can be defined.
    newsgroups_servers:
      - name: news.someserver.com
        username: foo
        password: bar
        connections: 10
      - name: eu.news.someserver.com
        username: foo
        password: bar
        connections: 10
    
  roles:
    - role: GR360RY.sabnzbd
```

HTPC-Ansible Project
--------------------

This role is part of HTPC-Ansible project that includes additional roles for building Ubuntu Based HTPC Server.

 Role name               | Comment
-------------------------|-----------------------------
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.htpc--common-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/htpc-common)   | Create htpc user and media folders
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.htpc--nas-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/htpc-nas)         | Configure NAS ( NFS, CIFS and AFP )
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.kodi--client-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/kodi-client)   | Install Kodi Media Player
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.kodi--mysql-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/kodi-mysql)     | Install MySQL Backend for Kodi
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.deluge-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/deluge)              | Install Deluge Bittornet Client
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.nzbtomedia-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/nzbtomedia)      | Install NZBtoMedia Postprocessing
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.sickrage-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/sickrage)          | Install SickRage
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.couchpotato-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/couchpotato)    | Install CouchPotato
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.htpc--manager-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/htpc-manager) | Install htpc-manager
<!--
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.sabnzbd-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/sabnzbd)            | Install Sabnzbd
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.tvheadend-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/tvheadend)        | Install Tvheadend

Additional Info is available at [www.htpc-ansible.org](http://www.htpc-ansible.org)
 -->
License
-------

BSD

Author Information
------------------

Gregory Shulov
