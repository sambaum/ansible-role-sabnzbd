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
4. Configure Categories if installed with `nzbtomedia` roles

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

# Usenet Crawler API key
usenet_crawler_api_key: 9d9af7ab6548948fcdc2db864ecd2588

# Sabnzbd Incomplete download locations
sabnzbd_incomplete: "{{ htpc_downloads_incomplete }}/sabnzbd"

# Helper vaiable. In use by other roles
sabnzbd_host: "{{ ansible_default_ipv4.address }}"

# Sabnzbd port. Default port 8080 conflicts with Kodi default port
sabnzbd_port: 9000
```

Dependencies
------------

Role Name | Description
----------|-----------
[![Galaxy](http://img.shields.io/badge/galaxy-GR360RY.htpc--common-blue.svg?style=flat-square)](https://galaxy.ansible.com/GR360RY/htpc-common/)| Create htpc user and media folders

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

Gregory Shulov
