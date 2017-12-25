# SSH Service
1. start/restart ssh service:
    ```bash
    service ssh start/restart
    ```
1. config: `/etc/ssh/sshd_config`
    ```
    PasswordAuthentication yes
    PermitRootLogin yes
    ```
1. auto start
    ```bash
    update-rc.d ssh enable
    ```
