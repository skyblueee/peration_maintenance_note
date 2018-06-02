# How to update Linux offline
## Use an iso file
### Debian
1. mount your iso file to a certain place.
(e.g. `#mount -o loop path/to/your.iso  /media/cdrom`)
1. use apt-cdrom to add to source (this will change file `source.list`).
(e.g. `#apt-cdrom -m -d /media/cdrom add`)
### CentOS
1. mount your iso file to a certain place.
    ```bash
    #mount -o loop path/to/your1st.iso  /media/cdrom1
    #mount -o loop path/to/your2nd.iso  /media/cdrom2
    ```
1. modify `/etc/yum.repos.d/CentOS-Media.repo`
    ```
    [cd-media]
    baseurl=file:///media/cdrom1
            file:///media/cdrom2
    enabled=1
    ```
1. Use it!
    * `#yum --enablerepo=cd-media [cmd]` to use this repo.
    * `#yum --disablerepo=\* --enablerepo=cd-media [cmd]` for __only__ this.
    * If bored with the long cmd, use `#yum clean all` and `#yum update`.

## Use local repository.
### Debian
1. make a local dir (e.g. `/var/debs`) and copy your deb files into it.
Maybe you want `rsync -zvrtopg usb /var/debs` to update.
1. `#dpkg-scanpackages debs /dev/null |gzip > /var/debs/Packages.gz`
1. add `deb file:///var/debs` to sources.list.

### CentOS
1. make a local dir (e.g. `/yum`) and copy your RPM files into it.
1. create a CentOS-Local.repo file.
    ```bash
    #cat >> /etc/yum.repos.d/CentOS-Local.repo <<EOF
    [Local]
    name=Local Yum
    baseurl=file:///yum/
    gpgcheck=1
    gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
    enabled=1
    EOF
    ```
1. `#createrepo /yum` and `#yum makecache`

## Setup a mirror of yourself
### Debian
1. Use `apt-mirror` to download a source list.
    1. `#apt-get install apt-mirror`
    1. modify `/etc/apt/mirror.list`, change `base_path`, add sources.
        ```
        set base_path = /path/to/your/aptUsbMirror
        deb http://mirrors.eg.com/debian dist main contrib non-free
        ```
    1. `#apt-mirror`
    1. `rsync -zvrtopg /path/to/your/aptUsbMirror /path/to/offline_mirror`
1. Visit your mirror.
    1. Local file visit. Modify source.list on localhost.
        ```
        deb file:///path/to/offline_mirror/mirror/mirrors.eg.com/debian dist main contrib non-free
        ```
    1. Http visit.
        1. Use Apache to set up a source mirror.
            1. `#ln -s /path/to/offline_mirror/mirror/mirrors.eg.com/debian /var/www/debian`
            1. modify `httpd.conf`.
                ```
                <Directory />
                    Options FollowSymLinks
                    AllowOverride None
                    Order deny,allow
                    allow from all
                </Directory>

                <Directory "/debian">
                      Options Indexes FollowSymLinks
                      AllowOverride None
                      Require all granted
                </Directory>
                ```
            1.  `#service apache2 restart`
        1. modify source.list on the client.
            ```
            deb [arch=amd64] http://server_ip/debian dist main contrib non-free
            ```
1. If GPG Error:
    `wget -q -O - https://archive.kali.org/archive-key.asc | apt-key add`

[More detail References](http://www.cnblogs.com/pengdonglin137/p/3474260.html)
