## Preparation
1. `java -version`(should be >=1.8)
1. install and configure mysqld, `vim /etc/mysql/conf.d/mysqld.cnf`
    ```
    collation_server=utf8_bin
    character_set_server=utf8
    skip-character-set-client-handshake
    default-storage-engine=INNODB
    max_allowed_packet=256M
    innodb_log_file_size=2GB
    binlog_format=row
    transaction-isolation=READ-COMMITTED
    ```
1. run `service mysql start & mysql` as root:
    ```mysql
    create database confluencedb default character set utf8 collate utf8_bin;
    create user 'confluence'@'localhost' identified by 'confluencepasswd';
    grant all privileges on confluencedb.* to 'confluence'@'%' identified by 'confluencepasswd';
    flush privileges;
    ```
## Installation
1. `wget https://www.atlassian.com/software/confluence/downloads/binary/atlassian-confluence-6.6.0-x64.bin`
1. `chmod +x atlassian-confluence-6.6.0-x64.bin`
1. `./atlassian-confluence-6.6.0-x64.bin`
1. `firefox localhost:8090`

## Crack[Test only]
1. `firefox localhost:8090` -> Production Installation -> Copy Server ID
1. `service confluence stop`
1. `cp /opt/atlassian/confluence/confluence/WEB-INF/lib/atlassian-extras-decoder-v2-3.2.jar atlassian-extras-2.4.jar`
1. run `java -jar Crack/confluence_keygen.jar` **on Windows**. Patch atlassian-extras-2.4.jar and generate the key.
1. `cp atlassian-extras-2.4.jar /opt/atlassian/confluence/confluence/WEB-INF/lib/atlassian-extras-decoder-v2-3.2.jar`
1. `cp Crack/mysql-connector-java-5.1.42-bin.jar /opt/atlassian/confluence/confluence/WEB-INF/lib`
1. `service confluence start`
1. `firefox localhost:8090` -> DatabaseURL=jdbc:mysql://localhost/confluence?sessionVariables=tx_isolation='READ-COMMITTED'

## Firewall
1. `firewall-cmd --zone=public --add-port=8090/tcp --permanent`
1. `firewall-cmd --reload`

## 附件预览中文乱码
1. `cp -r /your_path_to_win_C/windows/fonts /usr/share/fonts/msfonts`
1. Edit `/opt/atlassian/confluence/bin/setenv.sh`, add new line:
    > CATALINA_OPTS="-Dconfluence.document.conversion.fontpath=/usr/share/fonts/msfonts/ ${CATALINA_OPTS}"
1. `cd /var/atlassian/application-data/confluence/ & rm -rf viewfile/* & rm -rf shared-home/dcl-document/*`(clear cache for old files)
1. restart confluence.


## Customization
* [Disable attachment downloads](disable_attachment_downloads_confluence.md)
