# Ansible role for installing Logitech Media Server (LMS) 
An ansible role for installing and configuring the logitechmediaserver. Also referred to as LMS, slimdevices or squeezebox.

## Requirements
Ansible >= 2.11

# Example playbook
This example playbook installs LMS and the plugins Raopbridge (Air Bridge), CastBridge (Chromecast Bridge), Material Skin, Spotty and UPnPBridge.
Directory structure
```yaml
 l ansible.cfg
 l inventory
 l install_lms.yml
 l + files
   l + lms
     l CastBridge-2.2.8.zip
     l RaopBridge-1.3.3.zip
     l UPnPBridge-2.2.3.zip
     l Spotty-4.8.8.zip
     l lms-material-3.4.5.zip
     l logitechmediaserver_8.4.0~1694167045_amd64.deb
```
File: `ansible.cfg`:
```yaml
[defaults]
gathering = smart
fact_caching = jsonfile
fact_caching_connection = ~/.ansible/fact_cache
fact_caching_timeout = 84600
timeout = 60
inventory = inventory
```
File: `inventory`:
```yaml
[hosts]
myhost

[hosts:vars]
ansible_user=ansibleuser
ansible_become=yes
ansible_become_password=ansiblepassword
```
File: `install_lms.yml`:
```yaml
---
- name: "Install Logitechmediaserver  on myhost and configure it."
  hosts:
    - myhost

  tasks:
    - name: "[PLAYBOOK] - Install and configure LMS Logitech Media Server."
      ansible.builtin.include_role:
        name: marioqxx.lms
      tags:
        - lms
        - plugin
        - conf
        - repo
      vars:
        lms_server: files/lms/logitechmediaserver_8.4.0~1694167045_amd64.deb
        lms_plugins:
          - { name: CastBridge, file: files/lms/CastBridge-2.2.8.zip }
          - { title: "AirPlay bridge", file: files/lms/RaopBridge-1.3.3.zip }
          - { name: UPnPBridge, file: files/lms/UPnPBridge-2.2.3.zip }
          - { name: Spotty, file: files/lms/Spotty-4.8.8.zip }
          - { name: MaterialSkin, file: files/lms/lms-material-3.4.5.zip }
        lms_settings:
          - ["pref", "skin", "material"]
          - ["pref", "language", "DE"]
          - ["pref", "longdateFormat", "'%A, |%d. %B %Y'"]
          - ["pref", "shortdateFormat", "'%d.%m.%Y'"]
          - ["pref", "timeFormat", "'%H:%M'"]
          - ["pref", "mediadirs", ["/media/music", "/media/audiobook"]]
          - ["pref", "playlistdir", "/media/playlists"]
          - ["pref", "wizardDone", "1"]
          - ["pref", "plugin.castbridge:bin", "squeeze2cast-linux-x86_64-static"]
          - ["pref", "plugin.raopbridge:bin", "squeeze2raop-linux-x86_64-static"]
          - ["pref", "plugin.upnpbridge:bin", "squeeze2upnp-linux-x86_64-static"]
```
After having installed this ansible-role and setup the above directory stucture and placed the files and the file-contents as above, calling

`ansible-playbook install_lms.yml`

should install the logitechmediaserver ready to use without needing to step through the Wizard on you host `myhost` and with the above configured plugins and Material Skin activated.

### Minimal configuration with plugins
In a minimal example, you do not need to create the subdirectories files/lms and place any files. All will be downloaded automatically. The minimal configuration the simplifies to:

File: `install_lms.yml`:
```yaml
---
- name: "Install Logitechmediaserver  on myhost and configure it."
  hosts:
    - myhost

  tasks:
    - name: "[PLAYBOOK] - Install and configure LMS Logitech Media Server."
      ansible.builtin.include_role:
        name: marioqxx.lms
      vars:
        lms_plugins:
          - { name: CastBridge }
          - { title: "AirPlay bridge" }
          - { name: UPnPBridge }
          - { name: Spotty }
          - { name: MaterialSkin }
        lms_settings:
          - ["pref", "skin", "material"]
          - ["pref", "language", "DE"]
          - ["pref", "longdateFormat", "'%A, |%d. %B %Y'"]
          - ["pref", "shortdateFormat", "'%d.%m.%Y'"]
          - ["pref", "timeFormat", "'%H:%M'"]
          - ["pref", "mediadirs", ["/media/music", "/media/audiobook"]]
          - ["pref", "playlistdir", "/media/playlists"]
          - ["pref", "wizardDone", "1"]
          - ["pref", "plugin.castbridge:bin", "squeeze2cast-linux-x86_64-static"]
          - ["pref", "plugin.raopbridge:bin", "squeeze2raop-linux-x86_64-static"]
          - ["pref", "plugin.upnpbridge:bin", "squeeze2upnp-linux-x86_64-static"]
```
