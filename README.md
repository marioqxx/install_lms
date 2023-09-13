# lms
An ansible role for installing and configuring the logitechmediaserver. Also referred to as LMS, slimdevices or squeezebox.

## Requirements

# Example playbook
install_lms.yml

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
