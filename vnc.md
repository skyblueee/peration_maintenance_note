# VNC Service

## Kali
There is something wrong with gnome3 and vnc. use mate instead.
1. Install mate
    ```bash
    apt-get install mate-core mate-desktop-enviroment* mate-themes
    ```
1. Change program `/usr/bin/tightvncserver`,
    ```
    "/etc/X11/Xsession\n");
    ```
    to
    ```
    "unset DBUS_SESSION_BUS_ADDRESS\n"
    "/usr/bin/mate-session&\n");
    ```
1. Change `/etc/bash.bashrc`
    ```bash
    export XDG_RUNTIME_DIR="/run/user/$UID"
    ```
1. Change mod of `/run/user`
    ```bash
    chmod 777 /run/user
    ```

That's all. Use `$vncserver` to create a server,
and `$vncserver -kill :<num>` to delete one.
