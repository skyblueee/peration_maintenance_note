# Note about Peration and Maintenance

1. [How to update Linux offline](offline_source.md)
1. [How to setup SSH service](ssh.md)
1. [How to setup VNC service](vnc.md)
1. [How to setup Jira service](jira.md)
1. [How to setup Confluence service](confluence.md)

-------------
## Set dirs in HOME to be English
```bash
export LANG=en_US
xdg-usr-dirs-gtk-update  # Chose Yes
export LANG=zh_CN.UTF-8
xdg-usr-dirs-gtk-update  # Chose No and No more ask.
```
or edit file `~/.config/user-dirs.dirs`, mkdir, and relogin.
